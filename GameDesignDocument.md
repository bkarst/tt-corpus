# Trivia Tango - Comprehensive Game Design Document

## 1. Executive Summary
**Trivia Tango** is a mobile-first, highly interactive trivia and collection application designed to engage users through varied gameplay modes—spanning single-player exploration to real-time, competitive multiplayer features. The game combines a deep trivia experience with a robust progression system, social mechanics, a collectible "Venue" metagame, and a premium monetization framework. 

**Core Loop:** Answer trivia questions → Earn XP/Coins/Mystery Boxes → Expand the "Venue" collection & Unlock new categories → Challenge Friends/Climb Ratings.

---

## 2. Core Gameplay & Mechanics

### 2.1 Game Modes
1. **Single Player:**
   - Players navigate a 3-level hierarchy: **Mega Category** → **Category** → **Topic**.
   - Includes "Quick Play" (random from favorites) or "Play All Categories" options.
   - Topics can be unlocked using in-game currency (Coins).
2. **Standard Multiplayer:**
   - Matchmaking against random opponents or direct friend challenges.
   - Operates in three rounds, with a counter-pick category system when accepting a challenge.
3. **Quick Match (Auto-Match):**
   - A fast-start multiplayer variant bypassing manual category selection.
   - Questions are automatically pooled from the union of both players' favorite *Mega Categories*.
   - The challenger immediately plays Round 1 without waiting for the opponent to accept.

### 2.2 Scoring & Rounds (Multiplayer Example)
- **Round 1 (R1):** 4 questions with base values of 50, 75, 100, 125 pts (Max 350 pts).
- **Round 2 (R2):** 4 questions with base values of 100, 150, 200, 250 pts (Max 700 pts).
- **Round 3 (R3):** 5 questions with base values of 150, 200, 250, 300, and a final wager question (350 + wager pts). (Max 1,250+ pts).
- **Max Base Score:** 2,300 points (plus up to 50% wager bonus on the final question).
- **Risky Answer Mode (Optional Setting):** Imposes a -50 point penalty for wrong answers, but introduces an "I don't know" option to pass safely.

### 2.3 Question & Answer Mechanics
- **Format:** Multiple-choice format.
- **Hints System:** Players have a maximum of 3 hints per game to narrow down choices or provide clues.
- **Algorithm:** Matches question difficulty to the average player rating (Glicko), enforces a 24-hour question cooldown, and filters for high-quality questions (confidence > 80%).
- **Post-Question Content:** Players can view detailed explanations, bookmark questions for later, or interact with text-to-speech audio **"Story Cards"** that provide deeper historical/contextual background.

---

## 3. Economy & Progression System

### 3.1 Currencies
- **Coins:** The primary soft currency. Used to unlock categories, upgrade collection items, and purchase standard Mystery Boxes.
- **Gems:** The premium hard currency. Used for high-tier Store purchases, rare Mystery Boxes, or expediting timers.

### 3.2 Progression & Rewards
- **XP & Levels:** Players earn XP from games to level up. Level badges are displayed on player profiles and leaderboards.
- **Rewards Distribution (Multiplayer):**
  - **Win:** +50 XP, ×2.5 Coins, 1 Mystery Box.
  - **Tie:** +30 XP, ×1.5 Coins, 1 Mystery Box.
  - **Loss:** +20 XP, ×1.0 Coins, No Mystery Box.
- **Daily Missions & Streaks:** The Home dashboard tracks daily mission progress (e.g., "Play 3 History games") and consecutive win streaks, rewarding players with bonus coins.
- **Mystery Boxes:** Earned from winning games or purchased in the store. Boxes have unlock timers (e.g., 12 hours) and yield random rewards (Coins, Gems, Artifacts).

### 3.3 Skill Ratings
- The game uses a **Glicko Rating System** to determine player skill.
- Profiles display an Overall Rating, as well as distinct ratings broken down by Knowledge Area, Category, and Topic.

---

## 4. The Venue & Collection System

The "Venue" is Trivia Tango's idle/collection metagame, providing a long-term resource sink and visual representation of progress.

### 4.1 Artifacts
- **Rarity Tiers:** Common (Gray), Uncommon (Green), Rare (Blue), Epic (Purple), Legendary (Orange), Mythical (Red/Gold).
- **Maintenance (Cleanliness):** Artifacts degrade over time and require "Maintenance" (costing coins) to restore their cleanliness and value.
- **Actions:** Players can *Maintain* (restore cleanliness), *Upgrade* (improve base stats), or *Sell* artifacts for coins.

### 4.2 Venue Tiers
Players upgrade their physical Venue to hold more collections. Upgrading requires specific coin balances and completed collections. The upgrade path:
1. Locker → 2. Vault → 3. Gallery → 4. Hall → 5. Wing → 6. Exploratorium → 7. Museum → 8. Palace.

### 4.3 The Store
Located within the Venue hub, the Store allows players to purchase:
- Featured Items & Daily Deals
- Mystery Boxes of varying rarities
- New Category Unlocks
- Direct Artifact pieces

---

## 5. Social & Competitive Features

### 5.1 Friends System
- Players can search for friends by username or invite them via Email / Deep Links (`/invite/[token]`).
- The Friend Profile displays head-to-head stats, win rates, achievements, and allows for direct challenges.

### 5.2 Challenge Hub
- A dedicated view for managing received and sent challenges, organized by status: Pending, Ready, In-Game, and Completed.
- Multiplayer games feature real-time opponent score tracking and visual indicators of the opponent's status.
- End-of-game screens feature detailed score comparisons, question-by-question breakdowns, winner celebration animations (confetti), and immediate "Rematch" buttons.

---

## 6. Monetization

### 6.1 Trivia Tango Pro (Subscription)
A premium subscription tier offering:
- Completely Ad-Free experience.
- Monthly recurring rewards (e.g., 500 Coins, 50 Gems).
- Prominently displayed via a profile badge.
- Managed via the `/profile/subscription` and `/paywall` views.

### 6.2 In-App Purchases
- Users can purchase bundles of Coins and Gems directly from the Store to accelerate Venue progress and acquire premium Mystery Boxes.

---

## 7. App Flow & User Experience

The application is structured around a mobile-first, bottom-navigation design supporting responsive layouts, light, and dark themes.

### 7.1 Authentication & Onboarding
- **Auth:** Email/Password (with strict strength requirements) or Google OAuth.
- **Onboarding (New Users):** 
  1. Welcome screen for Display Name input.
  2. Category Selection requiring a minimum of 3 Mega Categories to personalize Quick Match pools.

### 7.2 Main Navigation Tabs
1. **Home:** Central hub featuring 4 Mystery Box slots (with unlock timers), daily missions progress, win streak card, and quick access buttons for Single/Multiplayer.
2. **Bookmarks/Content:** A repository for Saved Questions (with full explanations) and Liked Stories (with Text-to-Speech audio playback).
3. **Challenges:** The social hub for pending and active multiplayer games.
4. **Friends:** Search, add, and manage social connections and notifications.
5. **Venue/Profile:** Access to the collection metagame, Store, and personal statistics.

### 7.3 Accessibility & Settings
The settings menu (`/profile/settings`) offers deep customization:
- **Appearance:** Light / Dark / System themes.
- **Sound & Haptics:** Independent toggles for SFX and vibration.
- **Accessibility Settings:** 
  - *Extended Time:* Adds an extra 15 seconds per question.
  - *Text Size:* Default / Large / Extra Large.
  - *Reduce Motion:* Minimizes animations and confetti effects.
- **Content Management:** Manage "Hidden Topics" to prevent them from appearing in random games.
- **Account:** Options to reset the profile or permanently delete the account.

---

## 8. Views & User Interface Details

The application is composed of 34 distinct views grouped into logical categories. Below is a comprehensive breakdown of every view and its features.

### 8.1 Authentication Views

**`/login` (Sign In)**
- **Authentication:** Email/Password login, Google OAuth, Password visibility toggle, Remember Session.
- **Navigation:** Links to Forgot Password, Sign Up, Terms of Service, and Privacy Policy.
- **Feedback:** Email validation, inline error display, loading states during auth.

**`/signup` (Account Creation)**
- **Registration:** Email input, Password creation with strength rules, Confirm Password, Google OAuth.
- **Validation:** Email format, password matching, Terms/Privacy acceptance checkbox.
- **Feedback:** Inline errors, loading spinner, success redirect to `/welcome`.
- **Navigation:** Link to Sign In, Terms, Privacy.

**`/forgot-password` (Password Recovery)**
- **Recovery Flow:** Email input, reset link delivery via email, success confirmation message.
- **Feedback:** Email format validation, error display, loading state.
- **Navigation:** Back to Sign In.

### 8.2 Onboarding Views

**`/welcome` (Welcome)**
- **Profile Initialization:** Text field for Display Name (2-50 chars).
- **Google Pre-fill:** Auto-populates if signed in via Google.
- **UX:** Progress indicator ("Step 1 of 2"), loading state, validation checks.

**`/select-categories` (Category Selection)**
- **Selection:** Grid of 11 Mega Categories (e.g., History, Tech, Pop Culture).
- **Rules:** Multi-select; requires minimum of 3 categories to proceed.
- **UX:** Progress indicator ("Step 2 of 2"), visual selection badges, state persistence.

### 8.3 Main Application Views

**`/` (Home Dashboard)**
- **Header:** User Avatar, Name, Level Badge, XP Progress Bar, Coin Balance.
- **Daily Content:** Daily Missions (progress & claim modals), Claimable Achievements badge.
- **Mystery Boxes:** 4 box slots showing unlock timers; open button on completion.
- **Game Triggers:** "Play Single Player" and "Play Multiplayer" CTAs.
- **Status:** Pending challenges indicator and current Win Streak card.
- **Navigation:** Bottom tab bar.

### 8.4 Single Player Views

**`/play/single` (Single Player Setup)**
- **Selection Hierarchy:** Drill-down into Mega Category → Category → Topic. Includes coin unlock mechanics for premium topics.
- **Quick Play:** "Shuffle and Go" using favorite categories.
- **Search & Filter:** Global topic search, Recents Tab, Favorites Tab.

**`/play/single/recent` (Recent Plays)**
- **Play History:** Scrollable list of recently played topics/categories.
- **Actions:** Quick replay buttons, favorite toggles (star). Excludes unplayable mixed lists.

**`/game/[id]` (Active Game)**
- **Pre-Question (Story View):** Category breadcrumb, Story text/image, Audio controls (TTS), Topic rating (Like/Dislike/Hide), Continue button.
- **Question View:** Progress (X of Y), Difficulty badge, Question text with Read Aloud TTS, 4 multiple choice options.
- **Feedback View:** Correct/Incorrect banner with points earned, detailed explanation with TTS, post-question actions (Bookmark, Like/Dislike, Block).
- **Multiplayer Additions:** Real-time opponent score, online status indicator, Betting UI for wagers.

**`/results/[id]` (Single Player Results)**
- **Hero:** Trophy celebration, Result message ("Excellent!"), Large Final Score card, Stats (Accuracy & Correct count).
- **Rewards:** XP, Coins earned, Mystery Box claiming.
- **Breakdown:** Question-by-question review with earned points, correct answers, and "Like" buttons.
- **Follow-up:** Mission completion cards, Play Again topic carousels, Share/Home buttons.

### 8.5 Multiplayer Views

**`/play/multiplayer` (Matchmaking)**
- **Matchmaking:** "Find Match" CTA, animated searching state, cancel option.
- **Favorites:** Saved multiplayer game configurations.
- **Social:** Add Friend quick button.

**`/challenges` (Challenge Hub)**
- **Tabs:** Received and Sent challenges.
- **Status Sections:** Pending, Category Pending, Ready, In Game, Recent Results.
- **Challenge Cards:** Opponent avatar/name, Score ("You X - Y Opponent"), Win/Loss badge, Category icons, Timestamp.
- **Actions:** Accept/Decline, Start Game, Resume, View, Claim Rewards.

**`/challenges/new` (New Challenge)**
- **Step 1 (Opponent):** "From Friends" tab (scrollable list), "By Email" tab (invite non-friends), Tutorial modal.
- **Step 2 (Category):** Grid of all Mega Categories to set the game theme.
- **Creation:** Send Challenge button, loading state, redirect on success.

**`/challenges/[id]/accept` (Accept Challenge)**
- **Info:** Opponent display and the category they chose.
- **Counter-Pick:** Category grid allowing the player to select their counter-category.
- **Actions:** Accept (starts game) or Decline.

**`/results/[id]/multiplayer` (Multiplayer Results)**
- **Winner Display:** Winner banner, Confetti animation, Medal icons.
- **Comparison:** Your Score vs. Opponent Score, Score difference. Player cards displaying accuracy.
- **Breakdown:** Head-to-Head question review showing both players' answers and points.
- **Actions:** Rematch, Share, Home.

**`/multiplayer-game-complete/[id]` (Game Complete V2)**
- **State:** Final scores, Winner announcement, Opponent info.
- **Rewards:** Modal showing XP, Coins, Mystery boxes earned. Claim animation.
- **Actions:** Rematch and Home buttons.

### 8.6 Profile & Settings Views

**`/profile` (User Profile)**
- **Header:** Avatar, Display Name, Level Badge, Subscription Badge, XP Progress.
- **Statistics:** Games Played, Games Won, Accuracy, Win Streak, Win Rate, Best Topic.
- **Ratings:** Glicko overall rating, broken down by Knowledge Area, Category, and Topic.
- **Achievements:** "View All" button showing completion counts.
- **Quick Links:** Navigation to My Categories, Bookmarked Questions, Liked Stories, Settings.
- **Actions:** Sign Out button.

**`/profile/settings` (Settings)**
- **Appearance:** Theme toggle (Light/Dark/System).
- **Audio/Haptics:** Sound Effects toggle, Haptic Feedback toggle.
- **Accessibility:** Extended Time (+15s), Text Size (S/M/L), Reduce Motion.
- **Gameplay:** "Risky Answer Mode" toggle (-50 pts for wrong, "I don't know" skip).
- **Misc:** Tutorial replay, Subscription status, Sign Out.
- **Danger Zone:** Reset Profile, Delete Account.

**`/profile/subscription` (Subscription Management)**
- **Status:** Current plan card (Trivia Tango Pro vs. Free).
- **Billing:** Next renewal date, billing cycle.
- **Rewards:** Monthly Coin/Gem allocations.
- **Management:** Instructions for OS-level subscription modification, "Upgrade to Pro" CTA.

**`/categories` (My Categories)**
- **Selection:** Grid of Mega Categories to edit Quick Play preferences.
- **Validation:** Minimum 1 category must remain selected.

### 8.7 Social Views

**`/friends` (Friends Hub)**
- **Search:** Filter friends by name.
- **Requests:** Pending requests section with Accept/Decline actions.
- **Invitations:** "Invitations for You" and "Sent Invitations" sections with count badges.
- **Friends List:** List of accepted friends with Challenge and View Profile buttons.

**`/friends/add` (Add Friend)**
- **By Email:** Email input, Send Invite button.
- **By Username:** Search input (2+ chars minimum), User cards showing level and Add Friend/Pending status.

**`/friends/[id]` (Friend Profile)**
- **Info:** Friend's Avatar, Name, Level, and Core Stats (games, wins, accuracy).
- **Head-to-Head:** Win record against this friend and recent matches.
- **Achievements:** List of their earned badges.
- **Actions:** Challenge button, Remove Friend confirmation.

**`/notifications` (Notifications Center)**
- **Header:** Unread count badge, Mark All Read button.
- **Notification Types:** Challenge accepted/declined, Game results (Win/Loss/Tie), Round unlocks, Friend requests, Achievements.
- **Card UI:** Relevant icon, Title, Description, Timestamp, Unread indicator, Delete action.

**`/invite/[token]` (Invite Claim Flow)**
- **Logic:** Handles deep links. Identifies if the invite is for a Friend Request or a Game Challenge.
- **Handling:** Shows sender info; claiming rewards the user (25 gems) and sender (50 gems).
- **Fallbacks:** Handles expired/cancelled links, or "Share with friends" UI if tapping own link.

### 8.8 Content Views

**`/bookmarks` (Saved Questions)**
- **Tabs:** Bookmarked Tab and Liked Tab with counts.
- **Question Cards:** Question text, category badge, difficulty indicator, Bookmark/Like toggles, Expand button.
- **Expanded View:** Shows all answer options (correct highlighted), Explanation, and related Story.
- **Empty State:** Large icon with "Play a Game" CTA.

**`/stories/liked` (Liked Stories)**
- **Content:** Scrollable list of saved Story Cards (image + narrative).
- **Audio:** Full playback controls (Play, Pause, Mute, Progress bar).
- **Actions:** Expand/Collapse, Unlike button.

**`/topics/hidden` (Hidden Topics)**
- **Content:** List of topics blocked by the user.
- **Cards:** Topic name and parent category.
- **Action:** Unhide button to restore topic to random pools.

### 8.9 Collection (Venue) Views

**`/venue` (My Gallery Hub)**
- **Header:** Gallery level, Slot count, Coin balance, XP Progress.
- **Tabs:** Collections Tab (grouped by theme) and All Artifacts Tab.
- **Collections View:** Progress bars, "X / Y collected" counters, Completion badges.
- **Venue Progress:** Timeline showing upgrades (Locker → Vault → Gallery → Hall → Wing → Exploratorium → Museum → Palace). Checkmarks on completed tiers, requirements listed for next tier.

**`/venue/artifacts` (All Artifacts)**
- **Grid:** All owned artifacts with color-coded Rarity borders.
- **Filtering/Sorting:** By rarity, newest, cleanliness. Stats bar showing items needing maintenance.
- **Modal:** Tap to view large image, rarity, condition. Action buttons: Maintain, Upgrade, Sell.

**`/venue/store` (Store)**
- **Header:** Gem and Coin balances.
- **Tabs:** Featured, Mystery Boxes, Categories, Collection, Artifacts.
- **Cards:** Item previews, price badges (coins/gems).
- **Flow:** Purchase confirmation, insufficient funds warnings, success animations.

### 8.10 Subscription Views

**`/paywall` (Trivia Tango Pro)**
- **Benefits:** Ad removal, monthly coins (500), monthly gems (50).
- **Pricing:** Dynamic OS store pricing with Free Trial options.
- **Flow:** Native in-app purchase integration, Restore Purchases button, Loading/Success/Error states.

### 8.11 Legal Views

**`/terms` (Terms of Service)**
- **Content:** Full legal text with numbered sections (Acceptance, Conduct, Virtual Items, etc.).
- **Meta:** Last Updated date, Support Email link.

**`/privacy` (Privacy Policy)**
- **Content:** Detailed outline of data collection, usage, sharing, and security.
- **Meta:** Last Updated date, Contact Us section.