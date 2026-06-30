# Trivia Tango - Comprehensive Game Design Document

> **Source of truth note:** This document is reconciled against the shipped code in `../tt_flutter` (Flutter mobile client) and `../sage-next` (Next.js backend + API), as of **2026-06-30**. Where the client and backend differ, the **shipped Flutter client** is treated as canonical for player-facing behavior and the **backend** as canonical for mechanics/economy values. Items still needing confirmation are marked ⚠️.

## 1. Executive Summary
**Trivia Tango** is a mobile-first, highly interactive trivia and collection application. It pairs single-player exploration with three flavors of competitive multiplayer: **asynchronous turn-based duels**, and two **real-time "Live Play" modes** (First to Answer and Team Trivia). It combines a deep trivia engine (Glicko-2 adaptive difficulty, voiced Story Cards, hands-free audio play) with a robust progression stack (XP/levels, Daily Missions, Daily Streaks, 100+ Achievements), a collectible **Venue** metagame, time-boxed **Featured Events** with leaderboards and medals, and a premium monetization framework.

**Core Loop:** Answer trivia questions → Earn XP / Coins / Mystery Boxes → Expand the "Venue" collection, complete Missions/Streaks/Achievements, & climb Featured leaderboards → Challenge friends (or NPCs) asynchronously, gather a crew for **live games**, and climb Ratings.

---

## 2. Core Gameplay & Mechanics

### 2.1 Game Modes
1. **Single Player:**
   - Players navigate a 3-level hierarchy: **Mega Category** → **Category** → **Topic**.
   - Entry options (`PlaySettingsType`): **Quick Play** (random mix from favorite categories), **Category Play**, **Mega-Category Play**, **Topic Play**, and **Favorites Mix**.
   - Featured games are ordinary single-player games whose questions are scoped to the active Featured period and tagged for leaderboard credit; there is no separate "Featured Play" play-type. ⚠️ (GDD previously listed "Featured Play" as a distinct entry option — it is not.)
   - Topics can be unlocked using in-game currency (Coins; default ≈500 per topic).
2. **Standard Multiplayer (turn-based challenge):**
   - Direct friend challenges, random matchmaking, or **NPC opponents** (AI-controlled, rating-matched — see §6.4).
   - Two challenge flows: **`accept_decline`** (both players pick categories before play) and **`play_first`** (challenger plays Round 1 immediately; opponent joins and plays each round after).
   - Operates in three rounds (13 questions total); opponents progress asynchronously with **24-hour per-round timers** and an initial **48-hour response window** to first respond. Missed rounds are forfeited.
3. **Quick Match (Auto-Match):**
   - A fast-start turn-based variant bypassing manual category selection (`auto_pick_categories` + `challenge_and_play`).
   - Questions are automatically pooled from the union of both players' favorite *Mega Categories* (fallback to all if empty).
   - The challenger immediately plays Round 1 without waiting for the opponent to accept.
4. **Live Play (Real-Time):** Synchronous, scheduled multiplayer. Two modes are shipped behind feature flags — **Live First to Answer** and **Live Team Trivia** — and a third (**Live Multiplayer**) is spec'd but not yet built. See §3.

### 2.2 Scoring & Rounds (Turn-Based Multiplayer)
Config: `lib/config/multiplayer-scoring.ts` (`QUESTIONS_PER_ROUND = {1:4, 2:4, 3:5}`, 13 total).
- **Round 1 (R1):** 4 questions, base values 50, 75, 100, 125 pts (Max 350 pts).
- **Round 2 (R2):** 4 questions, base values 100, 150, 200, 250 pts (Max 700 pts).
- **Round 3 (R3):** 5 questions, base values 150, 200, 250, 300, and a final **wager** question (350 + wager). (Max 1,250+ pts.)
- **Wager:** On the final question the player may wager **0–50%** of their current score (`MAX_WAGER_PERCENTAGE = 0.5`).
- **Risky Answer Mode (optional setting):** −50 point penalty for wrong answers, with an "I don't know" option to pass safely (+0).

### 2.3 Question & Answer Mechanics
- **Formats (backend `QuestionType`):** `mcq`, `fitb` (fill-in-the-blank), `dynamic_reveal`, and `jumbled`. The shipped client primarily surfaces **multiple choice** (4 options, A/B/C/D); True/False prompts are authored as 2-choice MCQs (there is no dedicated `true_false` type). ⚠️ (GDD previously listed True/False as a distinct client format.)
- **Visual Questions:** Questions may carry an image (`image_url` + description), shown full-screen then shrunk to a thumbnail in a two-phase reveal.
- **Hints System:** Up to 3 hints per question; correct answers incur a **25 pt penalty per hint used** (`HINT_PENALTY_PER_HINT = 25`).
- **Difficulty:** 1–5 scale. **Single-player scoring is a flat 100 base points** per correct answer (`BASE_POINTS = 100`; the `questionDifficulty` parameter is deprecated in single-player scoring). The difficulty-scaled formula **`100 + 20 × (difficulty − 1)`** is implemented and live **only in the Live Play modes** (FTA / Team), not in single-player. ⚠️ (A single-player spec for difficulty-scaled points + a "worth N points" indicator exists in `tt_flutter/spec/reward-difficult-questions.md` but is not yet implemented.)
- **Selection Algorithm:** Matches question difficulty to the player's Glicko rating (targets ~68% success), enforces a 24-hour question cooldown, and filters for high-quality questions. See §4.3 for the rating model.
- **Post-Question Content:** Detailed explanations, bookmark, like/dislike, hide-topic, and voiced **"Story Cards"** (text-to-speech) providing deeper context.

### 2.4 Audio Play Modes
Each game can run in one of three modes (`AudioMode`):
- **Off** — standard visual play.
- **Autoplay** — TTS reads the question and choices; the player taps to advance.
- **Hands-Free** — TTS reads, then speech-to-text **listens for the spoken answer** and auto-advances. Enables eyes-free play.

---

## 3. Live Play — Real-Time Modes

Live Play is Trivia Tango's family of **scheduled, synchronous** multiplayer modes. Both shipped modes share a real-time architecture, a difficulty-scaled scoring base, placement-tier rewards, daily faucet caps, and an auto-friend growth hook. **Live games do not update Glicko ratings** (ratings are read to weight question selection, but only turn-based multiplayer writes them).

### 3.1 Real-Time Infrastructure
- **State:** DynamoDB is the authoritative store; a disposable **Firestore read-projection** mirrors live state to clients for push, with a lightweight **REST polling** fallback (`/scoreboard`). Transport is Remote-Config selectable (`*_game_transport` = `firestore` | `polling`).
- **No websockets.** Timing is driven by a **server-authoritative shared clock** plus **lazy advances**: each question window is deterministic from the start timestamp + position, and pass/reveal transitions fire idempotently on any client's ~1s tick when a deadline passes. Compute is stateless (Vercel serverless); crons handle reminders and reaping.
- **Clients** anchor countdowns to a server timestamp (`ServerClock`). Answer/vote ordering is resolved server-side via atomic conditional writes.

### 3.2 Live First to Answer (FTA)
*Flag: `live_fta_trivia_enabled` (off in release, on in debug). Status: backend + Flutter complete; E2E/CI goldens pending.*

- **What it is:** A scheduled, synchronous **2–5 player** individual race. **First player to answer correctly takes all the points**; a wrong answer deducts and re-opens the question for the rest. Ad-hoc (no persistent team); players **auto-friend** all co-enrollees on join.
- **Structure:** Two rounds (Round 2 worth **2×**) plus a **Final Wager** round (Jeopardy-inspired). Questions per round are host-set (5–60 each).
- **Start flow:** A registered host creates the game (start time, player cap 2–5, questions/round, per-question timer, subject), gets a **6-digit join code**, and is auto-enrolled as player #1. Others join by code (`triviatango://join-fta/{code}`), invite (push), or request (if approval required). Floor to run is **2 players**; below that at kickoff the game is cancelled.
- **The first-to-answer loop (per question):** Each eligible player may submit an option or tap "I don't know" (free, per-pass). The server atomically claims the first submission; if **correct** → +points and the question ends; if **wrong** → −points, that player is eliminated from the question, the option is struck for everyone, and a **new pass** opens for the rest. The question also ends when only one option remains, all players are eliminated, all remaining players pass, or the pass times out. A **live answer feed** shows each attempt resolving (avatar, choice, ✓/✗, point swing) and a live re-sorting scoreboard (negative scores allowed).
- **Default timing** (`fta-game-config.ts`, host-tunable): 20s per pass, 9s answer-buffer before choices appear, 8s reveal, 15s round break, 20s wager phase, **45s** final question (not a race), 1.5s late-answer grace.
- **Final wager:** Max = `max(floor(score × 0.5), 200)` so low scorers still get a real comeback; correct = +wager, wrong/none = −wager, no floor.
- **Rematch:** On the results screen, the first player to tap "Rematch" opens a ~10s window; if ≥2 join, a fresh game spins up with the same settings and new questions.

### 3.3 Live Team Trivia
*Flag: `live_team_trivia_enabled` (off in release, on in debug). Status: backend + Flutter complete & verified; Firestore push is a launch requirement.*

- **What it is:** A scheduled, synchronous **team-vs-team** pub-quiz. **Persistent teams** enroll; each team **votes** on every question (most votes wins) and teams rank on a shared leaderboard across two rounds + a Final Wager.
- **Teams (persistent):** Owner (transferable, sole disband authority), co-admins (manage roster, approve joins, schedule games), and members. Modeled as `TeamMembership` edges (like friendships) for scale. Default max size 30 (`team_max_size`); a user may own up to 10 teams (`team_max_owned_per_user`). Teams have a **reusable 6-digit join code** (with durable brute-force lockout). Joining a team **auto-friends** all existing members.
- **Game scheduling:** A host-team admin schedules a game (start 10 min–30 days out, questions/round 5–60, per-question timer, subject, optional per-team seat cap). Other teams enroll by code or request. Floor **1 team** (solo practice night), max **12 teams** per game.
- **Voting & resolution:** Each seated member votes an option or "I don't know" (changeable until the window closes; last write wins). At close, the team's answer = the option with the most votes; **absent members defer** (no vote weight transfer); a tie is broken by the **team leader's** vote, else random. The team leader is chosen in the lobby (default owner, reassignable before Q1, reserved a seat under seat caps). In-team tallies are visible live (the table argues); other teams' votes are redacted until reveal.
- **Scoring:** Round 1 base `100 + 20×(level−1)`; Round 2 the same **×2** ("Double Trivia"). Wrong / no answer = **0** (no deduction, unlike FTA). Final is wager-only, leader-set, max `floor(score × 0.5)`; correct = +wager, wrong = −wager.
- **Default timing** (`team-game-config.ts`): 20s voting window, 12s reveal, 15s round break, 20s wager phase, 30s final vote.
- **Seats & spectators:** If a per-team seat cap is set, members claim seats first-come (leader reserved); over-cap members spectate (see everything, cannot vote, earn nothing). Seats lock at kickoff.

### 3.4 Live Play Rewards
Both modes share placement-tier rewards (`rewards.ts`). Coins scale with the player's/team's correct-answer count:

| Placement | Tier (mult.) | Coins | Mystery Box |
|---|---|---|---|
| 1st (or tied) | Winner (6×) | `60 + 30 × correct` | Auto-opened 8h box + a bonus locked box if a slot is free |
| 2nd (or tied) | Tie (4×) | `40 + 20 × correct` | Auto-opened 8h box |
| 3rd+ | Loser (3×) | `30 + 15 × correct` | None |

- **Team mode** pays this **per participating member**.
- **Participation gate (firm):** FTA requires answering ≥1 question; Team requires casting ≥1 vote. Spectators, no-shows, and silent members earn nothing.
- **Faucet cap:** `FTA_DAILY_FULL_REWARD_CAP` / `TEAM_DAILY_FULL_REWARD_CAP` (default **5** full-reward games/day each); past the cap, games still play but award single-player-equivalent coins and no box.

### 3.5 Live Multiplayer (Planned, Not Implemented)
Spec'd in `tt_flutter/spec/live-multiplayer.md` but **no code yet**: a 2–4 player real-time mode with speed-weighted scoring (`base × speedFactor`, floor 0.5) and **NPC backfill** (10s wait for humans, then fill from the existing 251-account NPC engine). The turn-based matchmaking queue (`/api/matchmaking/*`) and NPC engine exist but are not wired to live play.

---

## 4. Economy & Progression System

### 4.1 Currencies
- **Coins:** Primary soft currency. Earned from games, Missions, Streaks, Achievements, mystery boxes, Featured rewards, and Live Play placements. Spent to unlock categories/topics, upgrade & maintain artifacts, upgrade the Venue, and refresh missions. **Earned-only — not purchasable via IAP.**
- **Gems:** Premium currency. Earned from Achievements (Silver+ tiers), Featured rewards, and friend-invitation bonuses; **purchasable via IAP gem packs.** Spent on store Mystery Boxes, early box-unlock, and mission refreshes.

### 4.2 XP, Levels & Multiplayer Rewards
- **XP & Levels:** `xpForLevel(N) = (N − 1)² × 100` → L2 = 100, L3 = 400, L10 = 8,100 (`lib/utils/index.ts`). Level badges display on profiles. ⚠️ (Corrected from the prior `N×(N−1)×50` formula.)
- **Per-answer / per-game:** 10 XP per correct answer, 25 XP per completed game.
- **Turn-based multiplayer outcome rewards:**
  - **Win:** +50 XP, Coins (~25 / win multiplier), Mystery Box (boosted artifact rolls).
  - **Tie:** +30 XP, Coins, Mystery Box (smaller boost).
  - **Loss:** +20 XP, base Coins, no box.

### 4.3 Skill Ratings (Glicko-2)
- The game uses a **Glicko-2** rating system on a scale **centered at 1500** (`DEFAULT_GLICKO_SCORE`). Ratings are maintained **independently per scope** — Topic, Category, Mega-Category, and Global each receive a full Glicko-2 update per result. ⚠️ (Corrected: there is **no** fixed 100%/40%/10% hierarchical cascade.)
- **Difficulty selection** uses an "effective score" that falls through **Topic → Category → Mega-Category → default**, picking the most specific scope whose rating deviation is confident (`RD < 220`). New users with no established scope rating default to an effective score of **1050** (`DEFAULT_USER_GLICKO_SCORE`), so they start on easy (level 1–2) questions until a real rating forms.
- Profiles display an Overall (Global) rating plus breakdowns by Category and Topic. Ratings feed difficulty selection, turn-based matchmaking, and the Featured "rating" leaderboard. **Live Play modes read but never write ratings.**

### 4.4 Daily Missions
- **Three rotating quests per day** (`DAILY_COUNT: 3`; e.g., "Play N games," "Win N games," "Answer N correctly," "Play N in category X").
- **Per-mission rewards:** Coins, and sometimes a Mystery Box (`DailyMissionRewardType = "coins" | "mystery_box"`). ⚠️ (Per-mission XP was previously documented; missions award coins/box, not XP.)
- **Refresh:** Swap a single mission for **2 Gems** (`MISSION_REFRESH_GEM_COST: 2`; limited per day).
- **Bonus:** Completing all three unlocks a bonus reward (Coins + XP).

### 4.5 Daily Streaks
*(Daily-**play** streak — playing any game each day; not a win streak.)*
- **Daily reward:** Coins scaling by tier — 20 (days 2–6), 30 (7–13), 40 (14–20), 50 (21–29), 60 (30–44), 70 (45–59), 80 (60–89), 90 (90–99), 100 (100+).
- **Milestones (one-time):** at **2, 7, 14, 21, 30, 45, 60, 90, 100, 180, 365** days, granting escalating Coins plus Mystery Boxes at the higher milestones (boxes appear from ~day 14+, e.g., day 21 grants 400 Coins + a 3-hour box). ⚠️ (Corrected: milestone list now includes days 2 and 21; boxes are milestone-attached, not "daily from day 4+".)
- Streak resets on a missed day.

### 4.6 Achievements
- **108 achievements** (`lib/config/achievements.ts`) across **6 categories**: `xp`, `mega_category` (category mastery), `single_player`, `multiplayer`, `streak`, and `mastery` (perfect games / lifetime correct answers).
- **Tiers & claim rewards:** Bronze (75 Coins) → Silver (200 Coins + 10 Gems) → Gold (500 Coins + 25 Gems) → Platinum (1,000 Coins + 50 Gems) → Diamond (2,500 Coins + 100 Gems), each with a tiered Achievement Mystery Box.
- **Claim-based:** auto-unlock on threshold, then the player **claims** to receive the rewards.

---

## 5. The Venue & Collection System

The "Venue" is Trivia Tango's idle/collection metagame — a long-term resource sink and visual record of progress.

### 5.1 Artifacts
- **Catalog:** **399 artifacts** (218 single-player, 181 multiplayer), distributed across mega-categories (`model/artifacts.yml`).
- **Rarity Tiers (counts):** Common/Gray (115), Uncommon/Green (134), Rare/Blue (81), Epic/Purple (48), Legendary/Orange (12), Mythical/Red-Gold (9).
- **Pieces → Levels:** Players collect pieces (from mystery boxes, streaks, rewards) to build and level artifacts (levels 1–5).
- **Maintenance (Cleanliness):** Artifacts degrade over time and require **Maintenance** (Coins) to restore condition/value.
- **Actions:** *Maintain*, *Upgrade* (advance a stage), or *Sell* for Coins.
- **Collections:** Artifacts group into named **Collections**; completing a collection grants a Coin-earning bonus and counts toward Venue upgrades.

### 5.2 Venue Tiers
Players upgrade their Venue to hold more collections. Each upgrade requires Coins (+ completed collections). Path and upgrade costs (`lib/config/venue-levels.ts`):
1. Locker (0) → 2. Vault (2,500) → 3. Gallery (7,500) → 4. Hall (20,000) → 5. Wing (50,000) → 6. Exploratorium (100,000) → 7. Museum (200,000) → 8. Palace (500,000).

### 5.3 Mystery Boxes
- **Slots:** Up to **4** concurrent mystery-box slots per user.
- **Earned boxes:** time-locked unlocks in three tiers — **3h / 8h / 12h** — yielding Coins + artifact-piece rolls of escalating rarity.
- **Store boxes (Gems):** Bronze (50) → Silver (100) → Gold (250) → Diamond (500), with higher rarity pools.
- **Early unlock:** spend Gems to skip the timer (≈1 gem per 30 min).

### 5.4 The Store
Accessible from its own bottom-nav tab and the Venue hub.
- **Backend store endpoint sells:** Mystery Boxes (Gems) and **Gem packs** (IAP). It does **not** sell coins.
- **Category / topic unlocks** are Coin purchases made at the point of play (dedicated unlock endpoint), surfaced in the single-player browse screen rather than as a Store SKU.
- ⚠️ Direct sale of individual artifacts and avatars: the client store view exposes tabs for these, but the backend store endpoint currently vends only boxes + gem packs; avatars/artifacts are primarily earned via boxes/achievements. Needs confirmation of which direct purchases are live.

---

## 6. Social & Competitive Features

### 6.1 Friends System
- Add friends by **username search** or **phone/contacts** (suggested friends surfaced from stored contacts).
- Friend profiles display level, core stats, and **head-to-head** records; support direct challenges.
- **Auto-friending:** Enrolling in a Live FTA game or joining a Team auto-creates bidirectional friendships with co-participants (idempotent), a deliberate growth hook.

### 6.2 Teams
Persistent team entities power Live Team Trivia (§3.3): roster with owner/co-admin/member roles, join-by-code or request-with-approval, top-performers display, and team management surfaces. See §3.3 for gameplay; §8 for the team notification set.

### 6.3 Invitations & Game Codes
- **Viral invites:** shareable link/deep-link (`/invite/:token`) and **6-digit game codes** (`/join/:code`, `/redeem-code`), with a shared code-generation + durable per-code lockout system reused across friend invites, team joins, and live-game joins.
- Invites can **pre-seed a multiplayer challenge** and grant **Gem rewards on the first game played together — sender 50 Gems, claimant 25 Gems** (30-day window).
- **Phone linking** auto-claims pending legacy invitations when a number is added.

### 6.4 Challenge Hub
- Manages received/sent challenges by status: Pending, Category-Pending, Ready, In-Game, Completed.
- Asynchronous opponent score tracking with round-by-round progress indicators.
- End-of-game screens: score comparison, question-by-question head-to-head breakdown, winner celebration (confetti), and Rematch.

### 6.5 NPC Opponents
- **AI-controlled opponents** (251 seeded accounts, `model/npc-accounts.ts`) that initiate challenges and play asynchronously, rating-matched to the player and carrying their own Glicko ratings. Used to keep the turn-based queue lively for new players.

---

## 7. Featured Events & Leaderboards

Time-boxed competitive periods (e.g., **"World History Week"**, typically **5–14 days**), scoped to a Topic or Category.
- **Three leaderboards:** highest single-game **Score**, highest **Rating** (min. 1 game), and most **Games** played.
- **Medals:** Rank 1 = Gold, Ranks 2–3 = Silver, Ranks 4–10 = Bronze — denormalized onto the player's profile for permanent display.
- **Milestones:** non-competitive Coin rewards for reaching game-count thresholds (**1 / 10 / 25 / 100** games). ⚠️ (Corrected from 1/3/5/10.)
- **Claim window:** ~30 days after a period finalizes.
- **Surfaces:** Home hero card with countdown, a Featured detail screen (standings, milestones, friends comparison), retention notifications, and **public SEO web pages** (`/app/featured/[periodId]`) with social/OG share cards.

---

## 8. Notifications
- **Push via Firebase Cloud Messaging (FCM)**; device tokens registered/cleared on login/logout.
- **~57 notification types** (`model/types/notification.ts`), all deep-linked, spanning:
  - **Challenge / turn-based multiplayer** (invited/accepted/declined; category-needed, game-ready, game-started, your-turn, round-unlocked, round-expiring, round/game-forfeited, game-complete, rewards-ready).
  - **Friends & invites** (request, accepted, invitation claimed, invite reward).
  - **Featured** (6 types: started, ending-soon, overtaken, on-the-bubble, reward-ready, claim-expiring).
  - **Teams** (~18 types: join request/approved/denied, member joined, removed, admin promoted, ownership transferred, plus team-game scheduled/cancelled/rescheduled/starting-soon/starting-now/result/enroll-request/approved/denied/team-removed).
  - **First to Answer** (~11 types: invite, scheduled, enroll request/approved/denied, player joined, starting-soon, starting-now, result, removed, cancelled/rescheduled).
  - **Other** (mystery-box ready, game result, system announcement).
- In-app Notifications Center mirrors these with read/unread state and per-item delete.

---

## 9. Monetization

### 9.1 Trivia Tango Pro (Subscription)
- **Product:** `com.triviatango.pro.monthly` (iOS + Android).
- **Price:** $3.99/month with a **1-week free trial** (auto-localized to all territories).
- **Benefits:** completely ad-free experience + a monthly grant of **500 Coins + 50 Gems**; a Pro badge on the profile.
- Surfaced via `/paywall` and `/profile/subscription`; renewals tracked via Apple App Store Server Notifications V2 and Google Play RTDN webhooks.

### 9.2 In-App Purchases
- **Gem packs** only (multiple denominations). Coins are **not** sold directly. Purchases are receipt-verified server-side and idempotent.

### 9.3 Advertising
- **Interstitial ads** before the results screen and **rewarded video** (optional, for Coins) for free users; cadence is controlled by Remote Config and fully removed by Pro.

---

## 10. App Flow & User Experience

Mobile-first, bottom-navigation design with responsive layouts and light/dark themes.

### 10.1 Authentication & Onboarding
- **Auth providers:** Email/Password, **Google**, **Apple**, and **Guest (anonymous)** play.
- **Guest mode:** instant play with an auto-generated handle; full feature parity; device-bound; an **upgrade path** (link email/social) preserves progress across devices. A profile banner prompts guests to verify/keep their progress.
- **Email verification:** conditionally required (Remote Config–gated); pre-verified for Google/Apple.
- **Onboarding:** display-name step, then category selection requiring **at least 1** Mega Category (personalizes Quick Play/Match pools); optional phone verification; optional inline game-code entry.

### 10.2 Main Navigation Tabs
Five bottom tabs:
1. **Home** — featured hero (with countdown), Mystery Box slots, Daily Missions & Streak cards, Continue Game, and Single/Multiplayer CTAs.
2. **My [Venue]** — the collection hub, labeled by the player's current Venue tier (e.g., "My Locker," "My Hall").
3. **Challenges** — pending/active multiplayer games.
4. **Store** — boxes, gem packs.
5. **Profile** — stats, ratings, achievements, medals, settings, subscription.

*(Friends, Notifications, Teams/Live Trivia, Bookmarks, and Liked Stories are reachable from Home/Profile rather than the tab bar.)*

### 10.3 Accessibility & Settings
`/profile/settings` offers: Appearance (Light/Dark/System), Sound & Haptics toggles, **Audio play mode** (Off/Autoplay/Hands-Free), Extended Time, Text Size, Reduce Motion, Hidden-Topics management, language, manage-subscription, and account actions (reset, delete).

---

## 11. Views & User Interface Details

The shipped Flutter client comprises **~59 GoRouter routes** (`lib/core/router/routes.dart`). Route params use the Flutter `:id` / `:code` / `:token` syntax.

### 11.1 Authentication & Onboarding (no bottom nav)
`/landing`, `/login`, `/signup`, `/forgot-password`, `/verify-email`, `/welcome`, `/select-categories`, `/phone-verification`.

### 11.2 Main Application (bottom-nav shell)
- **`/` (Home)**, **`/venue` (My [tier])** + `/venue/artifacts`, `/venue/store`, **`/store`**, **`/challenges`**, **`/friends`**, **`/profile`**.

### 11.3 Single-Player (full screen)
`/play/single`, `/play/single/recent`, `/game/:id`, `/results/:id`.

### 11.4 Turn-Based Multiplayer (full screen)
`/play/multiplayer`, `/challenges/new`, `/challenges/:id/accept`, `/multiplayer-game/:id`, `/multiplayer-game-complete/:id`, `/results/:id/multiplayer`.

### 11.5 Live Play (full screen)
- **Live Team Trivia:** `/live-trivia` (hub alias), `/teams`, `/teams/create`, `/teams/:id`, `/teams/:id/manage`, `/teams/:id/requests`, `/teams/:id/schedule`, `/team-games/:id`, `/team-games/:id/detail`, `/team-games/:id/results`, `/join-team/:code`, `/join-team-code`, `/join-game/:code`, `/join-game-code`.
- **Live First to Answer:** `/fta-games/create`, `/fta-games/:id`, `/fta-games/:id/detail`, `/fta-games/:id/results`, `/join-fta/:code`.

### 11.6 Featured (full screen)
`/featured/:id` — Featured period detail: your standing, 3 leaderboards, friends comparison, milestone & final reward claim.

### 11.7 Profile, Settings & Content
`/profile/settings`, `/profile/subscription`, `/paywall`, `/categories`, `/bookmarks`, `/stories/liked`, `/topics/hidden`.

### 11.8 Social (full screen)
`/friends/add`, `/friends/:id`, `/notifications`.

### 11.9 Invites & Deep Links
`/invite/:token`, `/join/:code`, `/redeem-code` (also via `triviatango://` deep links).

### 11.10 Legal
`/terms`, `/privacy`.

---

## 12. Related Documents
- `spec/battle-royale.md` — speculative "Trivia Royale" battle-royale concept for Live Play (not implemented).
- `../tt_flutter/spec/live-first-to-answer-trivia.md`, `live-team-trivia.md`, `live-multiplayer.md` — canonical Live Play specs.
- `../sage-next/docs/` — backend specs (ACHIEVEMENTS, STREAKS, ARTIFACT-REWARDS, NOTIFICATIONS, GAME_DESIGN).
