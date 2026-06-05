# Trivia Tango - Comprehensive Game Design Document

> **Source of truth note:** This document is reconciled against the shipped code in `../tt_flutter` (Flutter mobile client) and `../sage-next` (Next.js backend + API). Where the client and backend differ, the **shipped Flutter client** is treated as canonical for player-facing behavior and the **backend** as canonical for mechanics/economy values. Items still needing confirmation are marked ⚠️.

## 1. Executive Summary
**Trivia Tango** is a mobile-first, highly interactive trivia and collection application that pairs single-player exploration with asynchronous, turn-based competitive multiplayer. It combines a deep trivia engine (Glicko-2 adaptive difficulty, voiced Story Cards, hands-free audio play) with a robust progression stack — XP/levels, Daily Missions, Daily Streaks, and 100+ Achievements — a collectible **Venue** metagame, time-boxed **Featured Events** with leaderboards and medals, and a premium monetization framework.

**Core Loop:** Answer trivia questions → Earn XP / Coins / Mystery Boxes → Expand the "Venue" collection, complete Missions/Streaks/Achievements, & climb Featured leaderboards → Challenge friends (or NPCs) and climb Ratings.

---

## 2. Core Gameplay & Mechanics

### 2.1 Game Modes
1. **Single Player:**
   - Players navigate a 3-level hierarchy: **Mega Category** → **Category** → **Topic**.
   - Entry options: **Quick Play** (random mix from favorite categories), **Category Play**, **Topic Play**, **Favorites Mix**, and **Featured Play** (questions tied to the active Featured period).
   - Topics can be unlocked using in-game currency (Coins).
2. **Standard Multiplayer (turn-based challenge):**
   - Direct friend challenges, random matchmaking, or **NPC opponents** (AI-controlled, rating-matched — see §5.4).
   - Two challenge flows: **`accept_decline`** (both players pick categories before play) and **`play_first`** (challenger plays Round 1 immediately; opponent joins and plays each round after).
   - Operates in three rounds; opponents progress asynchronously with 24-hour per-round timers (missed rounds are forfeited).
3. **Quick Match (Auto-Match):**
   - A fast-start multiplayer variant bypassing manual category selection.
   - Questions are automatically pooled from the union of both players' favorite *Mega Categories* (fallback to all if empty).
   - The challenger immediately plays Round 1 without waiting for the opponent to accept.

### 2.2 Scoring & Rounds (Multiplayer)
- **Round 1 (R1):** 4 questions, base values 50, 75, 100, 125 pts (Max 350 pts).
- **Round 2 (R2):** 4 questions, base values 100, 150, 200, 250 pts (Max 700 pts).
- **Round 3 (R3):** 5 questions, base values 150, 200, 250, 300, and a final **wager** question (350 + wager). (Max 1,250+ pts.)
- **Wager:** On the final question the player may wager **0–50%** of their current score.
- **Risky Answer Mode (optional setting):** −50 point penalty for wrong answers, with an "I don't know" option to pass safely (+0).

### 2.3 Question & Answer Mechanics
- **Formats:** **Multiple choice** (4 options, A/B/C/D) and **True/False** in the shipped client. The backend additionally defines fill-in-the-blank, jumbled, and dynamic-reveal types ⚠️ (not all surfaced in the current client).
- **Visual Questions:** Questions may carry an image (`image_url` + description), shown full-screen then shrunk to a thumbnail in a two-phase reveal.
- **Hints System:** Up to 3 hints per question; correct answers incur a point penalty per hint used (≈25 pts each).
- **Difficulty:** 1–5 scale; single-player points scale with difficulty (≈ 100 + 20 × (difficulty − 1)).
- **Selection Algorithm:** Matches question difficulty to the player's Glicko-2 rating (targets ~68% success), enforces a 24-hour question cooldown, and filters for high-quality questions.
- **Post-Question Content:** Detailed explanations, bookmark, like/dislike, hide-topic, and voiced **"Story Cards"** (text-to-speech) providing deeper context.

### 2.4 Audio Play Modes
Each game can run in one of three modes:
- **Off** — standard visual play.
- **Autoplay** — TTS reads the question and choices; the player taps to advance.
- **Hands-Free** — TTS reads, then speech-to-text **listens for the spoken answer** and auto-advances. Enables eyes-free play.

---

## 3. Economy & Progression System

### 3.1 Currencies
- **Coins:** Primary soft currency. Earned from games, Missions, Streaks, Achievements, mystery boxes, and Featured rewards. Spent to unlock categories/topics, upgrade & maintain artifacts, upgrade the Venue, and refresh missions. **Earned-only — not purchasable via IAP.**
- **Gems:** Premium currency. Earned from Achievements (Silver+ tiers), Featured rewards, and friend-invitation bonuses; **purchasable via IAP gem packs.** Spent on store Mystery Boxes, early box-unlock, and mission refreshes.

### 3.2 XP, Levels & Multiplayer Rewards
- **XP & Levels:** Earned from games to level up (`XP_for_level_N = N × (N−1) × 50` → L2 = 100, L3 = 300, L10 = 4,500). Level badges display on profiles.
- **Per-answer / per-game:** ~10 XP per correct answer, ~25 XP per completed game.
- **Multiplayer outcome rewards:**
  - **Win:** +50 XP, Coins (~25 / win multiplier), Mystery Box (boosted artifact rolls).
  - **Tie:** +30 XP, Coins, Mystery Box (smaller boost).
  - **Loss:** +20 XP, base Coins, no box.

### 3.3 Skill Ratings (Glicko-2)
- The game uses a **Glicko-2** rating system, applied **hierarchically**:
  - **Topic** rating updates 100% per result, **Category** updates ~40% of the topic change, **Global** updates ~10%.
  - Default starting rating 1500; new players **inherit** from the parent category or global when a topic rating is missing.
- Profiles display an Overall (Global) rating plus breakdowns by Category and Topic. Ratings feed difficulty selection, matchmaking, and the Featured "rating" leaderboard.

### 3.4 Daily Missions
- **Three rotating quests per day** (e.g., "Play N games," "Win N games," "Answer N correctly," "Play N in category X").
- Rewards: Coins + XP (and sometimes a mystery box) per mission.
- **Refresh:** Swap a single mission for **2 Gems** (limited per day).
- **Bonus:** Completing all three unlocks a bonus reward.

### 3.5 Daily Streaks
*(Daily-**play** streak — playing any game each day; not a win streak.)*
- **Daily reward:** 20–100 Coins scaling with streak tier; a **Mystery Box from day 4+** (longer-timer boxes at higher tiers).
- **Milestones (one-time):** at 7, 14, 30, 45, 60, 90, 100, 180, 365 days, granting escalating Coins + boxes (up to thousands of coins at day 365).
- Streak resets on a missed day.

### 3.6 Achievements
- **100+ achievements** across **6 categories**: `xp`, `mega_category` (category mastery), `single_player`, `multiplayer`, `streak`, and `mastery` (perfect games / lifetime correct answers).
- **Tiers:** Bronze → Silver → Gold → Platinum → Diamond.
- **Claim-based:** auto-unlock on threshold, then the player **claims** to receive Coins, Gems (Silver+), and a tiered Achievement Mystery Box.

---

## 4. The Venue & Collection System

The "Venue" is Trivia Tango's idle/collection metagame — a long-term resource sink and visual record of progress.

### 4.1 Artifacts
- **Catalog:** ~399 artifacts (≈218 single-player, ≈181 multiplayer), distributed across mega-categories.
- **Rarity Tiers:** Common (Gray), Uncommon (Green), Rare (Blue), Epic (Purple), Legendary (Orange), Mythical (Red/Gold).
- **Pieces → Levels:** Players collect pieces (from mystery boxes, streaks, rewards) to build and level artifacts (levels 1–5).
- **Maintenance (Cleanliness):** Artifacts degrade over time and require **Maintenance** (Coins) to restore condition/value.
- **Actions:** *Maintain*, *Upgrade* (advance a stage), or *Sell* for Coins.
- **Collections:** Artifacts group into named **Collections**; completing a collection grants a Coin-earning bonus and counts toward Venue upgrades.

### 4.2 Venue Tiers
Players upgrade their Venue to hold more collections. Each upgrade requires Coins + completed collections. Path:
1. Locker → 2. Vault → 3. Gallery → 4. Hall → 5. Wing → 6. Exploratorium → 7. Museum → 8. Palace.

### 4.3 Mystery Boxes
- **Slots:** Up to **4** concurrent mystery-box slots per user.
- **Earned boxes:** time-locked unlocks in tiers (e.g., 3h / 8h / 12h), yielding Coins + artifact-piece rolls of escalating rarity.
- **Store boxes (Gems):** Bronze → Silver → Gold → Diamond, with higher rarity pools.
- **Early unlock:** spend Gems to skip the timer (≈1 gem per 30 min).

### 4.4 The Store
Accessible from its own bottom-nav tab and the Venue hub. Players can purchase:
- Mystery Boxes (Bronze→Diamond, Gems)
- Category / topic unlocks (Coins)
- Direct artifacts (Coins/Gems)
- Avatars (Coins/Gems)
- Gem packs (IAP)

---

## 5. Social & Competitive Features

### 5.1 Friends System
- Add friends by **username search** or **phone/contacts** (suggested friends surfaced from stored contacts).
- Friend profiles display level, core stats, and **head-to-head** records; support direct challenges.

### 5.2 Invitations & Game Codes
- **Viral invites:** shareable link/deep-link (`/invite/:token`) and a **6-digit game code** (`/join/:code`, `/redeem-code`).
- Invites can **pre-seed a multiplayer challenge** and grant **Gem rewards** on the first game played together.
- **Phone linking** auto-claims pending invitations when a number is added.

### 5.3 Challenge Hub
- Manages received/sent challenges by status: Pending, Category-Pending, Ready, In-Game, Completed.
- Asynchronous opponent score tracking with round-by-round progress indicators.
- End-of-game screens: score comparison, question-by-question head-to-head breakdown, winner celebration (confetti), and Rematch.

### 5.4 NPC Opponents
- **AI-controlled opponents** that can initiate challenges and play asynchronously (rating-matched to the player; production move delays ~3–15 min).
- Used to keep the multiplayer queue lively for new players; NPC accounts carry their own Glicko ratings.

---

## 6. Featured Events & Leaderboards

Time-boxed competitive periods (e.g., **"World History Week"**, typically 7–8 days), scoped to a Topic or Category.
- **Three leaderboards:** highest single-game **Score**, highest **Rating** (min. 1 game), and most **Games** played.
- **Medals:** Rank 1 = Gold, Ranks 2–3 = Silver, Ranks 4–10 = Bronze — denormalized onto the player's profile for permanent display.
- **Milestones:** non-competitive rewards for reaching game-count thresholds (e.g., 1/3/5/10 games).
- **Claim window:** ~30 days after a period finalizes.
- **Surfaces:** Home hero card with countdown, a Featured detail screen (standings, milestones, friends comparison), retention notifications (started, ending soon, overtaken, on-the-bubble, reward ready), and **public SEO web pages** with social/OG share cards.

---

## 7. Notifications
- **Push via Firebase Cloud Messaging (FCM)**; device tokens registered/cleared on login/logout.
- **~20 notification types**, deep-linked to the relevant screen, including: challenge invited/accepted/declined, multiplayer category-needed / game-ready / your-turn / round-unlocked / round-expiring / forfeited / game-complete / rewards-ready, friend request & accepted, invitation claimed, mystery-box ready, game result, Featured (period start/ending/overtaken/reward), and system announcements.
- In-app Notifications Center mirrors these with read/unread state and per-item delete.

---

## 8. Monetization

### 8.1 Trivia Tango Pro (Subscription)
- **Product:** `com.triviatango.pro.monthly` (iOS + Android).
- **Price:** $3.99/month with a **1-week free trial** (auto-localized to all territories).
- **Benefits:** completely ad-free experience + a monthly grant of **500 Coins + 50 Gems**; a Pro badge on the profile.
- Surfaced via `/paywall` and `/profile/subscription`; renewals tracked via Apple App Store Server Notifications V2 and Google Play RTDN webhooks.

### 8.2 In-App Purchases
- **Gem packs** only (multiple denominations). Coins are **not** sold directly. Purchases are receipt-verified server-side and idempotent.

### 8.3 Advertising
- **Interstitial ads** before the results screen and **rewarded video** (optional, for Coins) for free users; cadence is controlled by Remote Config and fully removed by Pro.

---

## 9. App Flow & User Experience

Mobile-first, bottom-navigation design with responsive layouts and light/dark themes.

### 9.1 Authentication & Onboarding
- **Auth providers:** Email/Password, **Google**, **Apple**, and **Guest (anonymous)** play.
- **Guest mode:** instant play with an auto-generated handle (e.g., "CuriousPanda"); full feature parity; device-bound; an **upgrade path** (link email/social) preserves progress across devices. A profile banner prompts guests to verify/keep their progress.
- **Email verification:** conditionally required (Remote Config–gated); pre-verified for Google/Apple.
- **Onboarding:** display-name step, then category selection requiring **at least 1** Mega Category (personalizes Quick Play/Match pools).

### 9.2 Main Navigation Tabs
Five bottom tabs:
1. **Home** — featured hero (with countdown), Mystery Box slots, Daily Missions & Streak cards, Continue Game, and Single/Multiplayer CTAs.
2. **My [Venue]** — the collection hub, labeled by the player's current Venue tier (e.g., "My Locker," "My Hall").
3. **Challenges** — pending/active multiplayer games.
4. **Store** — boxes, unlocks, avatars, gem packs.
5. **Profile** — stats, ratings, achievements, medals, settings, subscription.

*(Friends, Notifications, Bookmarks, and Liked Stories are reachable from Home/Profile rather than the tab bar.)*

### 9.3 Accessibility & Settings
`/profile/settings` offers: Appearance (Light/Dark/System), Sound & Haptics toggles, **Audio play mode** (Off/Autoplay/Hands-Free), Extended Time (+15s/question), Text Size (Default/Large/Extra Large), Reduce Motion, Hidden-Topics management, language, manage-subscription, and account actions (reset, delete).

---

## 10. Views & User Interface Details

The shipped Flutter client comprises ~40 routes (GoRouter). Below is the reconciled breakdown by area; route params use the Flutter `:id` / `:code` / `:token` syntax.

### 10.1 Authentication & Onboarding (no bottom nav)
- **`/landing`** — Welcome splash: Sign In, Create Account, **Play as Guest**.
- **`/login`** — Email/password + Google/Apple; forgot-password link.
- **`/signup`** — Account creation with password rules; Google/Apple.
- **`/forgot-password`** — Password reset email flow.
- **`/verify-email`** — Email verification gate (Remote Config–conditional).
- **`/welcome`** — Display-name entry (Google/Apple pre-fill).
- **`/select-categories`** — Mega-category multi-select (**≥1 required**).
- **`/phone-verification`** — Optional phone verification.

### 10.2 Main Application (bottom-nav shell)
- **`/` (Home)** — Featured hero + countdown, Mystery Box slots with timers, Daily Missions, Daily Streak card, Continue Game, claimable-achievements badge, Single/Multiplayer CTAs.
- **`/venue` (My [tier])** — Collections tab + All-Artifacts tab; Venue progress timeline (Locker→Palace) with completed checkmarks and next-tier requirements.
- **`/store`** — Featured / Mystery Boxes / Categories / Collection / Artifacts / Avatars; price badges (Coins/Gems), purchase + insufficient-funds flows; gem-pack IAP.
- **`/challenges`** — Received/Sent tabs; Pending, Category-Pending, Ready, In-Game, Recent Results; Accept/Decline, Start, Resume, View, Claim.
- **`/profile`** — Avatar, level, subscription badge, XP; stats (games, win rate, accuracy, best topic); Glicko ratings (overall/category/topic); achievements & **Featured medals**; quick links; sign out.

### 10.3 Single-Player (full screen)
- **`/play/single`** — Mega→Category→Topic drill-down, Quick Play ("Shuffle and Go"), search, Recents & Favorites tabs, coin-unlock for premium topics.
- **`/game/:id`** — Active game: Story view (TTS, like/dislike/hide), Question view (difficulty badge, Read-Aloud, MCQ/True-False, visual reveal), Feedback view (points, explanation, bookmark/like). Honors Off/Autoplay/Hands-Free modes.
- **`/results/:id`** — Trophy hero, final score, accuracy, XP/Coins, **mystery-box claim**, per-question review, mission-completion cards, Play-Again carousels, Share/Home.

### 10.4 Multiplayer (full screen)
- **`/play/multiplayer`** — Lobby: Quick Match / create challenge; saved configs; add-friend.
- **`/challenges/new?opponentId=`** — New challenge: opponent (friends / by-code) + category; `play_first` vs `accept_decline`.
- **`/challenges/:id/accept`** — Accept/decline; counter-pick category.
- **`/multiplayer-game/:id`** — Active 3-round game with async opponent status and final-question **wager UI**.
- **`/multiplayer-game-complete/:id`** — Completion/waiting state; rewards modal (XP/Coins/boxes); Rematch/Home.
- **`/results/:id/multiplayer`** — Winner banner + confetti, score comparison, head-to-head breakdown, Rematch/Share/Home.

### 10.5 Featured (full screen)
- **`/featured/:id`** — Featured period detail: your standing, 3 leaderboards, friends comparison, milestone & final **reward claim**.

### 10.6 Profile, Settings & Content
- **`/profile/settings`** — see §9.3.
- **`/profile/subscription`** & **`/paywall`** — Pro status, billing, benefits, native IAP, restore purchases.
- **`/categories`** — edit Quick Play mega-categories (≥1 must remain).
- **`/bookmarks`** — Bookmarked & Liked question tabs with expandable detail.
- **`/stories/liked`** — Saved Story Cards with full audio playback controls.
- **`/topics/hidden`** — Hidden topics with unhide action.

### 10.7 Social (full screen)
- **`/friends`** — friends list, requests, sent/received invitations.
- **`/friends/add`** — by username search or phone/contacts.
- **`/friends/:id`** — friend profile, head-to-head, challenge/remove.
- **`/notifications`** — center for the ~20 notification types; mark-all-read; delete.

### 10.8 Invites & Deep Links
- **`/invite/:token`** — claim a friend/challenge invite (rewards both parties with Gems); handles expired/own-link cases.
- **`/join/:code`** — redeem a 6-digit game code (also via `triviatango://join/{code}`).
- **`/redeem-code`** — standalone code entry.

### 10.9 Legal
- **`/terms`** — Terms of Service.
- **`/privacy`** — Privacy Policy.
