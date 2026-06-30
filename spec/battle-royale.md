# Trivia Royale — Speculative Spec (Live Play)

> **Status: SPECULATIVE / EXPLORATORY.** This is an idea spec, not committed work. It describes a proposed real-time "Live Play" mode for Trivia Tango modeled on Fortnite's Battle Royale. It is **not reconciled against shipped code** — unlike `../GameDesignDocument.md`, nothing here exists today. The current game is entirely asynchronous and turn-based; this mode is net-new. Treat everything below as a design sketch to react to, scope, and cut down.

## 1. Elevator Pitch

A live lobby of up to 100 players answers the same questions at the same time. Get one wrong, or answer too slowly once the pressure ramps, and you're out. Rounds compress, the field shrinks, and the last player standing takes the **Trivia Royale**. It's HQ Trivia's appointment-viewing energy rebuilt on Trivia Tango's adaptive engine, collection metagame, and seasonal economy.

This is the natural home for the existing `app-store/live-trivia.af` creative.

## 2. Why Fortnite Maps Cleanly

| Fortnite | Trivia Royale | Notes |
|---|---|---|
| 100-player Battle Royale | Up to 100-player live lobby | Last player standing wins |
| The Storm (shrinking circle) | The Storm (shrinking timer + correct-but-slow cuts) | Escalating pressure forces decisiveness |
| Victory Royale screen | "#1 Trivia Royale" win moment | Confetti pattern already exists for duels |
| Spectate after death | Spectate the leaders after elimination | Kills rage-quit; keeps lobby tense |
| Loot / rarity (Common→Legendary) | Power-up drops on existing artifact rarity tiers | Common→Mythical tiers already ship |
| Supply drops | Mid-round bonus question for a rare power-up or Venue piece | Feeds the idle/collection game |
| Squads / Duos | Team Trivia variant (see `app-store/team-trivia.af`) | Combined score + revive |

## 3. Core Loop & Format

1. **Drop in.** Player joins (or is matched into) a live lobby. A countdown gathers up to 100 players.
2. **Survive rounds.** Everyone answers the same question simultaneously under a shared timer.
3. **The Storm closes.** Each round, the timer shrinks and the elimination bar rises.
4. **Last player standing** wins Trivia Royale. Survivors place by elimination order.

### 3.1 Round & Elimination Mechanics

- **Shared questions, shared clock.** All surviving players see the same question and the same countdown. No per-player adaptive difficulty mid-lobby (it's a shared field), though the *lobby's* overall difficulty can ramp by round.
- **Early rounds (the field is wide):** wrong answer = eliminated. Correct = survive.
- **The Storm (mid/late rounds):** being correct is no longer enough. The slowest **X%** of correct answerers are also cut. X grows each round. This guarantees the field shrinks even if everyone is right, and it rewards speed + confidence the way the Storm rewards positioning.
- **Sudden death:** when ≤ N players remain, switch to head-to-head fastest-correct or a hardest-tier question gauntlet until one remains.
- **No hints in Royale** (or hints cost survival time), to preserve pace — contrast with single-player's 3-hint system.

### 3.2 Pacing Targets (placeholder, to be tuned)

| Phase | Players | Timer / question | Cut rule |
|---|---|---|---|
| Drop | up to 100 | 10s | wrong = out |
| Mid | ~40 | 8s | wrong = out, slowest 10% of correct = out |
| Storm | ~12 | 6s | wrong = out, slowest 25% of correct = out |
| Final | ≤4 | 5s | fastest-correct advances |

## 4. Lobbies & Matchmaking

Two complementary entry points:

- **Scheduled Showdowns (appointment events).** A global, everyone-at-once mega-lobby at a set time (the Travis-Scott-concert equivalent). Push the whole base via the existing FCM stack and Featured countdown UI. This is the marquee, retention-driving format.
- **On-Demand Royale.** A "drop now" queue that fills a lobby continuously so players never wait long. Backfill with NPC opponents (the existing rating-matched AI accounts) when the human queue is thin, same way the async multiplayer queue is kept lively today.

Matchmaking can optionally seed lobbies by **Glicko band** so casual and high-skill players aren't dumped together, mirroring a casual vs ranked split.

## 5. Power-Ups (In-Match Loot)

Net-new system — the game currently has no power-ups or lifelines beyond hints. Power-ups read instantly to players because they ride the existing artifact rarity tiers (Common → Mythical).

| Power-up | Effect | Fortnite analog |
|---|---|---|
| **Shield** | Survive one wrong answer this match | Shield potion |
| **50/50** | Remove two wrong options | — |
| **Freeze** | +X seconds on one question | — |
| **Double or Nothing** | Double placement reward if correct, eliminated if wrong | Risk/reward loot |
| **Steal** | Knock out a random rival if you answer first | Aggressive play |

- **Acquisition:** brought in as a pre-match loadout (limited slots) and/or grabbed from **supply drops** — a random high-value bonus question mid-round that yields a power-up or a Venue artifact piece.
- **Anti-pay-to-win guard:** power-ups should be earnable via play, not just purchasable, to keep Royale fair. Cosmetics carry the monetization weight (see §8).

## 6. Spectate

When eliminated, the player drops into **spectator mode** watching the remaining leaders finish, rather than being dumped to Home. Benefits:

- Reduces rage-quit and keeps eliminated players in the session.
- Builds tension toward the Victory Royale moment.
- Teaches — you see how top players answer fast questions.
- Optional: **spectate a friend** still alive in the lobby.

## 7. Victory & Placement

- **Victory Royale screen:** loud, branded "#1" celebration (reuse the duel confetti win-screen pattern), a shareable card, and permanent profile flair.
- **Placement rewards** scale by finish position (top 1 / top 3 / top 10 / survived N rounds), not just the winner, so most of the lobby earns something.

## 8. Economy & Progression Integration

Royale should feed the systems that already exist rather than spawn parallel ones.

- **Currencies:** placement pays **Coins** (earned-only soft currency) and **Gems** (premium) on the same scale as multiplayer outcomes today. Winning a Scheduled Showdown can grant a **Mystery Box** with boosted rolls.
- **XP & Levels:** Royale games award XP into the existing level curve.
- **Glicko-2:** a Royale placement updates ratings (or feeds a separate **Ranked Royale ladder** / Arena season). The hierarchical topic/category/global model already exists.
- **Featured Events:** Scheduled Showdowns can be scoped to the active Featured topic, making Royale the live centerpiece of a Featured week and reusing the leaderboard/medal denormalization already on profiles.
- **Daily Missions / Streaks:** add Royale-flavored missions ("Survive to the final 10," "Win a Royale") feeding the existing daily loop.
- **Battle Pass (cross-ref):** if a seasonal Battle Pass ships, Royale placement is a primary XP source for the track. Cosmetics (avatar skins, answer/victory emotes, win-screen banners) are the monetization layer and extend the existing Store/avatars.

## 9. What This Requires That Doesn't Exist Yet

The async architecture gives us almost none of the real-time plumbing. Net-new infrastructure:

- **Real-time room/lobby server** with synchronized question pacing across up to 100 clients (shared clock, authoritative server-side scoring).
- **Live matchmaking** for both scheduled and on-demand fill, including NPC backfill.
- **Spectator streaming** of lobby state to eliminated players.
- **Power-up system** (loadout, in-match items, supply-drop questions) — no power-up/lifeline framework exists today.
- **Anti-cheat / latency fairness** — simultaneous scoring makes timing and trust harder than the async duel flow.
- **Scheduled-event orchestration** beyond the current Featured countdowns.

**This real-time stack is the gating dependency for the entire Live Play vision** (Team Trivia, ranked Arena, live events all sit on top of it). Scope it once, then layer modes.

## 10. Reuses What Already Ships

Lowering the cost: artifact **rarity tiers** (power-up tiers), **Glicko-2** (ranked ladder), **FCM notifications + Featured countdowns** (appointment events), **avatars + Store** (cosmetics), **XP / Daily Missions / Featured medals** (progression), **confetti win screen** (Victory Royale), and **NPC opponents** (lobby backfill).

## 11. Open Questions

- Field size: is 100 realistic for our DAU, or start at 20–50?
- Adaptive difficulty inside a shared lobby — flat per round, or scaled to the surviving field's skill band?
- Do hints exist at all in Royale, and at what survival cost?
- Are power-ups purchasable, or strictly earned, to protect competitive integrity?
- How do we handle disconnects mid-lobby (grace reconnect vs. instant elimination)?
- Scheduled Showdown cadence — daily, weekly, or tied to Featured periods only?
- Does Team Trivia (squads + revive) ship alongside or after solo Royale?

## 12. Related

- `../GameDesignDocument.md` — canonical current (async) mechanics this builds on
- `app-store/live-trivia.af`, `app-store/team-trivia.af` — existing creative for the mode
- Sibling speculative ideas (Battle Pass, Team Trivia, Ranked Arena) — to be written
