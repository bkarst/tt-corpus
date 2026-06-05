# Trivia Tango - Branding Guide

This is how **Trivia Tango** looks, sounds, and talks — everywhere. App, store listings, ads, landing page, social, PR, support. If the GDD says *what* the game is, this doc says *how the brand sounds*.

---

## 1. Brand Overview

### 1.1 What We Are
Trivia Tango is a trivia game that's actually fun. Battle friends, crack boxes, collect cool stuff. That's it.

### 1.2 Mission
Make trivia the best part of your day.

### 1.3 Brand Pillars
1. **Smart** — We love people who love knowing things.
2. **Fun** — Games should feel like games. Don't forget that.
3. **Fair** — Good duels. Good rewards. No cheap shots.
4. **Surprising** — Every question, every box, every story has a twist.

### 1.4 Who It's For
Anyone who likes trivia. Whether you want to beat your friends, collect rare stuff, or just learn something weird on the bus.

---

## 2. Brand Personality & Voice

### 2.1 Who We Are
Trivia Tango is the friend who:
- Knows a ton of weird facts.
- Hypes you up when you win.
- Says "so close!" when you don't.
- Is always down for one more round.

### 2.2 Voice Principles
- **Friendly.** Like texting a friend, not reading a manual.
- **Short.** Five words beat fifteen. Every time.
- **Fun.** A little joke. A little warmth. Don't overthink it.
- **Clear.** If a 10-year-old wouldn't get it, rewrite it.

### 2.3 Tone Matrix
Tone flexes with context. Voice stays the same.

| Context | Tone | Example |
|---|---|---|
| Onboarding | Friendly, easy | "Pick 3 subjects you like. We'll find you a good match." |
| Winning | Happy, quick | "Nice one!" |
| Losing | Warm, light | "So close! Another round?" |
| Error | Chill, direct | "Something broke. Try again?" |
| Push | Short, fun | "Your box is ready." |
| Paywall | Honest, simple | "No ads. Monthly treats. Cancel anytime." |
| PR / press | Clear, friendly | Short sentences. No jargon. |
| Social | Playful, casual | Like a friend posting, not a brand posting. |

---

## 3. Logo & Wordmark Guidelines

### 3.1 Primary Wordmark
"**TRIVIA TANGO**" in our display typeface. Bold and simple. The two T's lean in toward each other — like two players facing off.

### 3.2 Usage Rules
- Maintain clear space equal to the cap-height of the "T" on all sides.
- Minimum digital size: 80 px wide. Minimum print size: 0.75 inches wide.
- On dark backgrounds, use the ivory variant. On light backgrounds, use the midnight variant.
- Never recolor, stretch, skew, rotate, add drop shadows, or place the wordmark on busy imagery without a scrim.

### 3.3 Icon Mark
A "TT" monogram. Two T's linked together. Used for the app icon, favicons, and social avatars. Always on a Tango Red background unless going monochrome.

---

## 4. Color Palette

### 4.1 Primary Brand Colors

| Name | Hex | RGB | Usage |
|---|---|---|---|
| **Tango Red** | `#D4145A` | 212, 20, 90 | Primary brand color. Logos, CTAs, accent moments. |
| **Midnight Stage** | `#0E1033` | 14, 16, 51 | Primary dark backgrounds, dark mode surfaces. |
| **Spotlight Ivory** | `#FAF5EC` | 250, 245, 236 | Primary light backgrounds, body text on dark. |
| **Arena Gold** | `#F5B93D` | 245, 185, 61 | Trophies, victory states, premium accents. |

### 4.2 Secondary Colors

| Name | Hex | Usage |
|---|---|---|
| **Velvet Plum** | `#5B2A86` | Supporting brand color, premium surfaces (paywall, subscription). |
| **Rhythm Teal** | `#1BB8A5` | Interactive accents, links, positive system states. |
| **Chalk Gray** | `#6B7280` | Secondary text, disabled states. |
| **Floor Black** | `#0A0A14` | Overlays, modal scrims. |

### 4.3 Rarity Tier Colors
Reserved exclusively for artifacts and Mystery Box loot reveals. Never use for generic UI.

| Tier | Hex | Token |
|---|---|---|
| Common | `#9CA3AF` | `rarity.common` |
| Uncommon | `#22C55E` | `rarity.uncommon` |
| Rare | `#3B82F6` | `rarity.rare` |
| Epic | `#A855F7` | `rarity.epic` |
| Legendary | `#F97316` | `rarity.legendary` |
| Mythical | `#E11D48` → `#F5B93D` gradient | `rarity.mythical` |

### 4.4 Functional Colors

| Name | Hex | Usage |
|---|---|---|
| Success | `#16A34A` | Correct answers, confirmations. |
| Warning | `#EAB308` | Maintenance alerts, timer warnings. |
| Error | `#DC2626` | Incorrect answers, failed transactions. |
| Info | `#2563EB` | Tips, tutorial cues. |

### 4.5 Color Ratios
Approximate balance across a typical screen:
- 60% neutrals (Midnight Stage or Spotlight Ivory)
- 25% supporting (Velvet Plum, Rhythm Teal, Chalk Gray)
- 10% Tango Red (brand moments, primary CTAs)
- 5% Arena Gold (reserved for celebration, premium)

### 4.6 Contrast & Accessibility
All text/background pairings must meet **WCAG 2.2 AA** at minimum (4.5:1 for body text, 3:1 for large text and UI). Audited pairings:

| Foreground | Background | Ratio | Pass |
|---|---|---|---|
| Spotlight Ivory | Midnight Stage | 16.2:1 | AAA body |
| Arena Gold | Midnight Stage | 10.1:1 | AAA body |
| Rhythm Teal | Midnight Stage | 6.8:1 | AA body |
| Tango Red | Spotlight Ivory | 5.1:1 | AA body |
| Tango Red | Midnight Stage | 3.3:1 | **Large text / UI only** |
| Velvet Plum | Spotlight Ivory | 7.9:1 | AAA body |

**Rule:** Tango Red never carries body copy on dark surfaces. For Tango Red CTAs on Midnight Stage, always set button label text in Spotlight Ivory — never in Tango Red over Tango Red backgrounds without the Ivory fill.

**Colorblind safety:** Correct/incorrect states (Success green / Error red) are always paired with a second cue — a check/cross icon or distinct copy. Never rely on color alone to communicate state. Rarity tiers likewise pair color with tier labels; a deuteranopic player can distinguish Common from Legendary because the label reads "Legendary."

**Reduce Motion:** Every celebratory animation (confetti, Mystery Box burst, Venue tier reveal) must ship with a tasteful static alternative that still communicates the win.

---

## 5. Typography

### 5.1 Type System

| Role | Typeface | Weight | Usage |
|---|---|---|---|
| **Display** | Fraunces | 600–900 | Headlines, scores, game-state announcements. High-drama moments only. |
| **Heading** | Inter | 700 | Section titles, card titles, modal headers. |
| **Body** | Inter | 400–500 | Paragraphs, captions, question text. |
| **Mono** | JetBrains Mono | 500 | Rating numbers, timers, Glicko stats. |

### 5.2 Type Scale (Mobile)
- H1 (Display): 36 / 40
- H2: 28 / 34
- H3: 22 / 28
- Body Large: 17 / 24
- Body: 15 / 22
- Caption: 13 / 18
- Micro: 11 / 14

### 5.3 Type Rules
- Never mix more than two typefaces on a single surface (Mono excluded for stat callouts).
- Display type gets tracked **-0.01em**; everything else stays at 0.
- Question text is set in Body Large with extra line-height for readability during timed play.
- Never full-justify body text. Left-align in LTR languages; right-align in RTL.

---

## 6. Messaging Hierarchy

### 6.1 Primary Tagline
> **Outsmart. Collect. Dominate.**

Three words. Three things to do. Our main tagline. Use it everywhere: landing page hero, App Store subtitle, top of launch ads.

### 6.2 Secondary Taglines (By USP)

| USP | Tagline |
|---|---|
| Trivia Meets Collection | "Play trivia. Collect cool stuff." |
| Auto-Match Intelligence | "Tap. Play. Done." |
| True Skill Tracking | "See how smart you really are." |
| The Learning Never Stops | "Every answer's got a story." |

### 6.3 Situational Taglines
Short phrases for specific moments.

- Competitive: *"Think you're good? Prove it."*
- Collector: *"Crack boxes. Collect treasures."*
- Learner: *"Weird facts, every round."*
- Social share: *"Who's up for a round?"*
- Push / reactivation: *"Your turn!"*
- Reward: *"You earned it."*
- Daily mission: *"3 missions. Let's go."*

### 6.4 Messaging Pillars → Proof Points

| Pillar | Message | Proof |
|---|---|---|
| Smart | "Real skill ratings. Not fake stats." | Glicko ratings by topic and category. |
| Fun | "Quick matches. No waiting around." | Turn-based duels, auto-match, wagers. |
| Fair | "Every box is worth opening." | Six rarity tiers. No pay-to-win. |
| Surprising | "Trivia you'll actually remember." | Story Cards with the wild truth behind every answer. |

---

## 7. Blurbs & Descriptions

### 7.1 One-Liner (under 80 chars)
> Battle friends. Crack boxes. Collect cool stuff.

### 7.2 Short Blurb (≤ 30 words)
> Trivia Tango is a trivia game where you battle friends, open boxes, and collect rare stuff. Easy to play. Hard to put down.

### 7.3 Medium Blurb (≤ 60 words)
> Trivia Tango is trivia like you've never played it. Battle friends in quick turn-based matches. Open Mystery Boxes and collect rare artifacts. Listen to the wild true stories behind the answers. Play one round. Play a hundred. You'll keep coming back.

### 7.4 Long Description (App Store / Play Store)
See `AppStore.md` for the long description. Any updates must be mirrored in both places.

### 7.5 Press Boilerplate
> **About Trivia Tango** — Trivia Tango is a mobile trivia game. Battle friends in quick turn-based matches, open Mystery Boxes, and collect rare artifacts for your Venue. Every answer comes with a short story about the topic. Thousands of questions across 11 categories. Available on iOS and Android.

---

## 8. Language Do's and Don'ts

### 8.1 Vocabulary
**Use:** play, win, nail it, battle, duel, round, match, box, story, friend, rival, earn, collect, rare, cool, nice, close, whoops, let's go, bring it.

**Avoid:** curate, metagame, ecosystem, gamified, leverage, optimize, utilize, engagement, journey, experience (as a noun), commence, initiate, thou shalt.

Basically: use words you'd actually say out loud.

### 8.2 Writing Do's
- Lead with a verb. "Battle a friend" beats "You can now battle a friend."
- Keep it short. If you pass 15 words, cut.
- Talk to the player, not about them. Say "you," not "users."
- Be playful. A little joke beats a clean sentence.

### 8.3 Writing Don'ts
- Don't talk down. "Oops, silly you!" is out.
- Don't guilt-trip. "Don't lose your streak!" is out.
- Don't use ALL CAPS except for a final score.
- Don't pile on emoji. One per push max. None in most UI.
- Don't sound like a manual. "To initiate a match…" is out.

### 8.4 Example Rewrites

| Before | After |
|---|---|
| "Oops! Wrong answer :(" | "Nope! Here's the story." |
| "You LOST! Try again?" | "So close! Rematch?" |
| "Don't miss out on your Mystery Box!" | "Your box is ready." |
| "Congratulations!!! You WIN!!" | "Nice one!" |
| "Earn sweet rewards fast!" | "Win stuff. Fast." |
| "Commence your trivia journey" | "Let's play." |
| "Challenge your friends in epic duels" | "Take on a friend." |
| "Unlock legendary artifacts" | "Get rare stuff." |
| "You have successfully signed in" | "You're in." |
| "No match found. Please try again." | "Nobody's around. One sec?" |

---

## 9. Imagery & Illustration Style

### 9.1 Principles
- **Rich, warm colors.** Think cozy game night, not neon arcade.
- **Stuff looks real.** Artifacts should feel like you could pick them up and hold them.
- **Clean and simple.** One thing at a time. No crowded shelves.
- **Hands, not faces.** Show a hand cracking a box, not a face reacting. Faces don't travel across markets.

### 9.2 Photography
Skip the stock photos of friends laughing at a phone. Use real app screens or clean product shots instead.

### 9.3 Don't
- No generic "people holding phones" stock.
- No AI art in the app without a review.
- No cartoon or chibi artifact art. They need to feel like real treasures.

---

## 10. Iconography

- **Style:** Rounded, 2 px stroke, 24 px grid. Filled variants for selected/active states.
- **Family:** Custom icon set extending Lucide. Never mix multiple icon libraries on a single surface.
- **Rarity glints:** Mythical and Legendary artifact icons may include a subtle animated highlight. All other tiers remain static.
- **Category icons:** Each of the 11 Mega Categories has a bespoke icon — these are brand assets and cannot be swapped for generic equivalents.

---

## 11. Motion & Animation

### 11.1 Principles
- **Confident, not frantic.** Ease-out curves, 200–400 ms durations for most UI. No bouncy over-corrections.
- **Celebration is earned.** Confetti, screen shakes, and fanfare are reserved for match wins, Mystery Box openings, and Venue tier-ups — never for daily logins or minor interactions.
- **Respect Reduce Motion.** Every celebratory animation must have a dignified static alternative.

### 11.2 Signature Moments
- **Card flip** on post-question reveal: 350 ms, 3D Y-axis flip. Success green for correct. Chalk Gray for wrong.
- **Mystery Box crack:** 2-stage — tap to rattle (180 ms), tap again to burst open (650 ms, Arena Gold radial light).
- **Venue upgrade:** Slow reveal (1.2 s) with a camera dolly across the new tier. One of the most emotional moments in the product — give it room to breathe.

---

## 12. Audio Identity

### 12.1 Principles
Warm, fun, a little exciting. Not a game-show buzzer. Not a slot machine.

### 12.2 Signature Sounds
- **Correct answer:** A happy "ding" — two notes going up.
- **Wrong answer:** A soft low note. No buzzer. No "wah-wah."
- **Mystery Box opens:** A rising shimmer, then a satisfying pop.
- **Match win:** A short, happy fanfare ending with our audio logo.
- **Audio logo:** A 1.5-second two-note stinger at app launch and end of ads.

### 12.3 Rules
- Respect the Sound Effects toggle. Silent mode means silent.
- Story Card voice is warm and friendly. Like a friend telling you a cool fact — not a narrator.

---

## 13. Persona-Specific Messaging

Lead with the angle that matches the audience. The USP doesn't change — the emphasis does.

### 13.1 Competitive Brainiac (18–35)
- **Lead hook:** "Think you're smart? Prove it."
- **Featured mechanics:** Skill ratings, counter-picks, wagers.
- **Tone:** Direct. A little cocky. Confident.
- **Channels:** Reddit, X, YouTube quiz channels.

### 13.2 Collector / Completionist (25–45)
- **Lead hook:** "Crack boxes. Collect cool stuff."
- **Featured mechanics:** Venue tiers, rare artifacts, box openings.
- **Tone:** Excited, satisfying, a little addictive.
- **Channels:** TikTok, Instagram, collector subreddits.

### 13.3 Casual Learner (30–55+)
- **Lead hook:** "Play. Learn something weird."
- **Featured mechanics:** Story Cards, audio, Quick Play.
- **Tone:** Warm, curious, chill.
- **Channels:** Podcasts, Facebook, history/science YouTube.

---

## 14. Localization Notes

The Trivia Tango brand was built around an English-language wordplay (Trivia → "Trivia Tango"). As the app scales to the 5 priority markets (US, UK, Canada, Australia, Germany) and beyond, the brand must flex without losing identity.

### 14.1 What Travels
- **Tango Red, Midnight Stage, and Arena Gold** are universal. They read as premium, dramatic, and celebratory in every market we've tested.
- **The duel format** is universal. Head-to-head competition translates cleanly.
- **Rarity tiers and Mystery Box visual language** are already loanwords in global mobile gaming.

### 14.2 What Needs Care
- **"Tango" as a verbal metaphor.** In Spanish-speaking markets (Argentina especially), tango is a profound cultural institution — leaning on "floor," "steps," and "dance" can feel either cute or presumptuous depending on tone. In these markets, dial back the dance metaphors; let the product speak for itself.
- **"Outsmart. Collect. Dominate."** The word *Dominate* in German (*Dominieren*) and several East Asian languages carries harder, more aggressive connotations. Approved alternates: *Outsmart. Collect. Prevail.* or *Outsmart. Collect. Triumph.*
- **Humor and puns.** Our English voice leans on dry wit. Wit rarely localizes word-for-word — prioritize *tone match* over literal translation.
- **Honorifics.** Languages with formal/informal registers (DE: du/Sie, JP: casual/敬語) default to informal — we're a game, not a bank — but subscription, billing, and legal copy uses the formal register.

### 14.3 Rule
Localized copy is reviewed by a native speaker who has played the game. Machine translation is a starting draft, never a shipping asset.

---

## 15. Brand Architecture

### 15.1 Trivia Tango (Master Brand)
Default voice, colors, and tone as defined in sections 2–6. All marketing surfaces lead with master-brand identity unless specifically in a Pro context.

### 15.2 Trivia Tango Pro (Premium Tier)
Pro is a sub-brand. Not a separate brand. Same system, with three tweaks:
- **Accent color:** Velvet Plum `#5B2A86` replaces Tango Red on Pro surfaces (paywall, subscription page, Pro badge).
- **Voice:** Straightforward. No hype. "No ads. Monthly treats. Cancel anytime." — not "Join the elite."
- **Icon:** A small Arena Gold star next to the player's name. No crowns. No VIP tags.

### 15.3 Do Not Create Sub-Brands Without Cause
"Trivia Tango Kids," "Trivia Tango Classroom," or other extensions require explicit brand-lead sign-off. Sub-brand proliferation dilutes the core. If the new audience can be served by a feature, ship a feature — not a sub-brand.

---

## 16. Competitive Differentiation

How we sound different from the other guys.

| Competitor | Their vibe | Our vibe |
|---|---|---|
| **Trivia Crack** | Loud. Cartoony. Party-at-a-clown. | Chill. Friendly. Actually fun to come back to. |
| **HQ Trivia** | Game-show host yelling into a mic. | You and a friend, any time. |
| **Kahoot** | Classroom. Neon. Homework energy. | Grown-ups welcome. No teacher in sight. |
| **QuizUp** | Minimalist. Dry. | Warm. Playful. Plus cool stuff to collect. |
| **Duolingo** | "Don't lose your streak or else." | We invite. We don't threaten. |

**Internal one-liner:** "Trivia you actually want to play."

---

## 17. Channel Guidelines

### 17.1 App Store & Google Play
Copy is locked to `AppStore.md`. Screenshots follow the 7-screenshot plan there. Never A/B test copy changes in one store without mirroring the test in the other.

### 17.2 Landing Page
Copy locked to `LandingPage.md`. Hero always leads with "Outsmart. Collect. Dominate." Never swap in a secondary tagline as the H1.

### 17.3 Social Media

**Profile Bios:**
- TikTok / Instagram: `Flex your brain. Duel your friends. https://www.triviatango.app/`

**Video Sign-Off:**
- "For thousands more questions that match your skill level, download our app, Trivia Tango. Link in Bio."

| Platform | Voice adjustment | Cadence |
|---|---|---|
| X / Twitter | Sharp, topical, conversational | 4–6 posts/week |
| Instagram | Visual-first, aspirational | 3 posts/week + Stories daily |
| TikTok | Playful, trend-aware, never cringe | 3 videos/week |
| Reddit | Authentic, no marketing-speak | Monthly AMAs, weekly dev updates |
| YouTube Shorts | Fast-cut gameplay, educational angle | 2 per week |

### 17.4 Email & Push
- Push: max 35 characters title, 100 characters body. One per day per user (non-transactional).
- Email: one headline, one image, one CTA. Newsletter at most weekly.
- Never send a notification whose only purpose is "come back." Always lead with a concrete reason (ready box, friend's challenge, new category).

### 17.5 Support & Reviews
- Respond to 1★/2★ reviews within 48 hours.
- Never argue. Acknowledge, offer a path forward, invite to email support.
- Sign responses "— The Trivia Tango Team," never a first name.

---

## 18. When In Doubt

Stuck? Run it through these four questions. First "no" is your answer.

1. **Is it fun?**
   If it feels like a chore, rewrite it. We're a game, not a form.

2. **Is it short?**
   If you can cut a word, cut it. Then try cutting one more.

3. **Would a 10-year-old get it?**
   If a kid wouldn't understand it, a tired adult on the bus won't either.

4. **Does it translate?**
   If it only works in English wordplay, it's a nice-to-have. Not a core line.

Pass all four? Ship it. Flunk one? Revise. Flunk two? Start over.

### 18.1 Tie-Breakers
- **Clear beats clever.** Every time.
- **Less beats more.** Cut a word. Cut a color. Cut a step.
- **Their time beats our ego.** Don't make them wait for our joke.

---

## 19. Legal & Trademark

- **Trivia Tango** is a trademark. First mention in press, legal, or formal contexts uses ™ (or ® if and when registered).
- Never permit third parties to use the wordmark without written agreement.
- Influencers and affiliates may use approved marketing assets from the press kit — they may not modify, recolor, or re-typeset the logo.
- All content (questions, Story Cards, artifact art) is owned or licensed — never encourage or accept user-submitted content without a licensing agreement in place.

---

## 20. Quick Reference Card

Pin this above your desk.

- **Tagline:** Outsmart. Collect. Dominate.
- **Pillars:** Smart. Fun. Fair. Surprising.
- **Voice:** Friendly. Short. Fun. Clear.
- **Primary color:** Tango Red `#D4145A`
- **Background:** Midnight Stage `#0E1033`
- **Display type:** Fraunces 600–900
- **Body type:** Inter 400–500
- **North-star question:** Is it fun?
- **Never:** talk down, guilt-trip, pile on jargon, write long when short works.
- **Always:** keep it friendly, keep it short, keep it fun.

---

*Last updated: 2026-04-21. Changes to this guide require sign-off from the brand lead and a mirror-check against `AppStore.md`, `LandingPage.md`, and `GTMStrategy.md`.*
