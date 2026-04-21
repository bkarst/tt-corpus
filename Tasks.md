# Trivia Tango - GTM Execution Tasks

This document breaks down every task required to execute the Go-To-Market Strategy outlined in `GTMStrategy.md`. Each task includes scope, dependencies, and is rated on difficulty (1-10) and priority (High/Medium/Low).

---

## 1. User Research & Persona Validation

### 1.1 Conduct User Research Interviews
**Difficulty:** 4/10  
**Priority:** High

**Scope:**
- Recruit 15-20 participants across the three persona types (Competitive Brainiac, Collector/Completionist, Casual Learner)
- Design interview scripts focusing on trivia habits, collection game engagement, and learning motivations
- Conduct 30-45 minute remote interviews via Zoom or similar
- Synthesize findings into a research report validating or refining persona assumptions

**Deliverables:**
- Interview script document
- Participant recruitment criteria
- Research synthesis report with persona refinements

**Dependencies:** None

---

### 1.2 Build Persona Documentation & Share with Team
**Difficulty:** 2/10  
**Priority:** Medium

**Scope:**
- Create polished persona cards (1-pagers) for each of the three user types
- Include demographics, motivations, pain points, feature affinity, and messaging angles
- Distribute to marketing, product, and design teams for alignment

**Deliverables:**
- 3 persona card PDFs
- Internal presentation deck

**Dependencies:** Task 1.1

---

## 2. Messaging & Creative Development

### 2.1 Finalize USP Messaging Framework
**Difficulty:** 5/10  
**Priority:** High

**Scope:**
- Refine the four USPs ("Trivia Meets Collection", "Auto-Match Intelligence", "True Skill Tracking", "The Learning Never Stops") into a messaging hierarchy
- Create primary taglines, secondary value props, and proof points for each
- Test messaging with a small focus group or survey (n=50-100)
- Document approved messaging in a brand guidelines appendix

**Deliverables:**
- Messaging framework document
- Focus group/survey results summary

**Dependencies:** Task 1.1 (persona validation informs messaging)

---

### 2.2 Produce Video Ad Creatives (Venue Progression)
**Difficulty:** 7/10  
**Priority:** High

**Scope:**
- Write 3-5 video ad scripts (15s, 30s variants) showcasing the Venue progression from Locker to Palace
- Storyboard each video with screen recordings, motion graphics, and voiceover direction
- Coordinate with a video production team or agency to produce final assets
- Export in formats required by Meta, TikTok, Google UAC, and Apple Search Ads
- Create 2-3 variations per platform for A/B testing

**Deliverables:**
- Video ad scripts and storyboards
- 10-15 final video assets (platform-specific)
- Asset naming convention document for UA team

**Dependencies:** Task 2.1 (messaging), Game build with Venue feature complete

---

### 2.3 Develop Playable Ad Unit
**Difficulty:** 8/10  
**Priority:** High

**Scope:**
- Design a 15-30 second playable ad experience where users answer 2-3 trivia questions
- Correct answers "unlock" a Mystery Box with a rewarding animation
- End screen drives to app store with clear CTA
- Build using a playable ad SDK (e.g., Luna, Mintegral) or HTML5
- QA across iOS and Android ad network requirements (Meta, ironSource, Unity Ads)
- Iterate on drop-off points using early analytics

**Deliverables:**
- Playable ad design spec
- Final HTML5/SDK playable unit
- QA report across networks

**Dependencies:** Task 2.1 (messaging), Core gameplay loop finalized

---

### 2.4 Design Shareable Results Cards
**Difficulty:** 4/10  
**Priority:** Medium

**Scope:**
- Design highly visual, social-media-optimized result cards for end-of-game screens
- Include head-to-head scores, winner celebration, player avatars, and branding
- Optimize dimensions for Instagram Stories (1080x1920), X/Twitter (1200x675), and TikTok
- Implement native share functionality in-app (iOS Share Sheet, Android Intent)
- Add branded watermark and app download CTA to shared images

**Deliverables:**
- Figma designs for 3 share card variants
- Engineering spec for share implementation
- QA verification on iOS and Android

**Dependencies:** Multiplayer results screen implemented in-app

---

## 3. Deep Link Invite System

### 3.1 Implement Deep Link Infrastructure
**Difficulty:** 6/10  
**Priority:** High

**Scope:**
- Select and integrate a deep linking provider (Branch, Appsflyer, Firebase Dynamic Links)
- Configure Universal Links (iOS) and App Links (Android) for the `/invite/[token]` route
- Handle deferred deep linking for users who don't have the app installed (route through app store, then resume invite flow post-install)
- Implement token generation, validation, and expiration logic on the backend
- Build fallback web page for unsupported environments

**Deliverables:**
- Deep link provider integration
- Backend API for invite token CRUD
- QA test matrix covering installed/not-installed, iOS/Android, various browsers

**Dependencies:** Backend API, App build with invite claim view (`/invite/[token]`)

---

### 3.2 Build Referral Reward Fulfillment System
**Difficulty:** 5/10
**Priority:** High

**Scope:**
- Implement backend logic to credit 50 Gems to the sender and 25 Gems to the claimant upon successful invite redemption
- Prevent abuse: limit rewards per sender (e.g., max 20 invites/month), detect duplicate accounts
- Send push notification and in-app notification to sender when their invite is claimed
- Add referral tracking to analytics (who invited whom, conversion rate)

**Deliverables:**
- Referral reward API endpoints
- Abuse prevention rules document
- Analytics event schema for referrals

**Dependencies:** Task 3.1, In-app economy system

---

## 4. App Store Optimization (ASO)

### 4.1 Implement App Store Listing Copy
**Difficulty:** 2/10  
**Priority:** High

**Scope:**
- Upload finalized copy from `AppStore.md` to App Store Connect and Google Play Console
- Localize title, subtitle, and description for top 5 markets (US, UK, Canada, Australia, Germany)
- Ensure keyword density is optimized without keyword stuffing
- Submit and monitor for approval

**Deliverables:**
- Live App Store and Google Play listings
- Localized copy for 5 markets

**Dependencies:** App Store / Play Store developer accounts, Final app build

---

### 4.2 Produce App Store Screenshots
**Difficulty:** 5/10  
**Priority:** High

**Scope:**
- Design 7 screenshots per the plan in `AppStore.md` (Core Gameplay, Multiplayer, Venue, Home, Categories, Profile, Story Cards)
- Create device-framed mockups for iPhone 6.7", iPhone 6.5", iPad Pro, and Android phone/tablet
- Add bold captions at the top of each image per the screenshot plan
- Export in required resolutions and upload to stores

**Deliverables:**
- 7 screenshots x 4 device sizes = 28 image assets
- Figma source files

**Dependencies:** Near-final app UI, Task 2.1 (messaging for captions)

---

### 4.3 Set Up A/B Screenshot Testing
**Difficulty:** 4/10  
**Priority:** Medium

**Scope:**
- Use Google Play Experiments and/or a third-party tool (SplitMetrics, StoreMaven) for iOS
- Create two screenshot orderings: Variant A leads with "Core Gameplay", Variant B leads with "Venue Collection"
- Run test for 2-4 weeks or until statistical significance
- Analyze conversion rate lift and implement winning variant

**Deliverables:**
- A/B test configuration in Play Console / SplitMetrics
- Test results report with recommendation

**Dependencies:** Task 4.2 (screenshots complete)

---

### 4.4 Create App Preview Video
**Difficulty:** 6/10  
**Priority:** Medium

**Scope:**
- Produce a 15-30 second app preview video for App Store and Google Play
- Capture in-app gameplay footage showcasing core loop, multiplayer, and Venue
- Add motion graphics, music, and captions (no voiceover for global appeal)
- Export in required formats (App Store: 1920x1080 or device-specific; Play Store: landscape)

**Deliverables:**
- App preview video (2 versions: iOS and Android)
- Raw footage archive

**Dependencies:** Near-final app build

---

## 5. Paid User Acquisition (UA) Setup

### 5.1 Set Up Ad Accounts & Tracking
**Difficulty:** 5/10  
**Priority:** High

**Scope:**
- Create and configure ad accounts on Meta (Facebook/Instagram), TikTok, Google UAC, and Apple Search Ads
- Integrate Mobile Measurement Partner (MMP) SDK (Adjust, Appsflyer, or Singular) into the app
- Configure postback URLs and SKAN/Privacy Sandbox for iOS attribution
- Set up conversion events: Install, Registration Complete, First Game Played, First Purchase
- Verify data flow in MMP dashboard

**Deliverables:**
- Active ad accounts on 4 platforms
- MMP SDK integrated and tested
- Conversion event documentation

**Dependencies:** App build, MMP contract signed

---

### 5.2 Define Audience Targeting Strategy
**Difficulty:** 4/10  
**Priority:** High

**Scope:**
- Build initial audience segments based on persona research:
  - Interest targeting: Trivia Crack, Jeopardy, Lumosity, Elevate, idle games, brain training
  - Lookalike audiences seeded from beta testers or soft launch users
  - Demographic targeting aligned with persona age ranges
- Document targeting strategy per platform (Meta, TikTok, Google, Apple)
- Plan budget allocation across segments for soft launch testing

**Deliverables:**
- Audience targeting playbook (per platform)
- Initial budget allocation plan

**Dependencies:** Task 1.1 (personas), Task 5.1 (ad accounts)

---

### 5.3 Launch & Optimize UA Campaigns
**Difficulty:** 7/10  
**Priority:** High

**Scope:**
- Launch initial campaigns with video and playable ad creatives across all platforms
- Monitor CPI, CTR, and downstream retention daily during first 2 weeks
- Pause underperforming creatives (CPI > target) and scale winners
- Iterate on new creative variants every 2 weeks based on learnings
- Coordinate with analytics to monitor LTV:CAC ratio

**Deliverables:**
- Live campaigns on 4 platforms
- Weekly UA performance reports
- Creative iteration log

**Dependencies:** Tasks 2.2, 2.3 (creatives), Task 5.1, Task 5.2

---

## 6. Retention & Engagement Systems (In-App)

### 6.1 Implement Daily Missions System
**Difficulty:** 6/10  
**Priority:** High

**Scope:**
- Design 10-15 daily mission templates (e.g., "Play 3 History games", "Win 2 multiplayer matches", "Maintain 1 artifact")
- Build backend logic to assign 3 random missions per day at midnight (user local time)
- Track mission progress in real-time and update Home dashboard UI
- Implement mission completion rewards (Coins) and claim flow
- Add streak tracking: consecutive days with all missions completed

**Deliverables:**
- Mission template database
- Backend mission assignment and tracking API
- Frontend Home dashboard integration
- QA test plan

**Dependencies:** Core gameplay loops, Economy system

---

### 6.2 Build Mystery Box Timer & Notification System
**Difficulty:** 5/10  
**Priority:** High

**Scope:**
- Implement countdown timers for Mystery Box unlocks (configurable: 4h, 8h, 12h tiers)
- Build push notification triggers: "Your Mystery Box is ready!" at timer completion
- Add "Watch ad to halve timer" rewarded video integration (AdMob, ironSource)
- Design and implement box opening animation and reward reveal
- Handle edge cases: app backgrounded, time zone changes, clock manipulation

**Deliverables:**
- Timer logic (backend + frontend sync)
- Push notification integration
- Rewarded video ad integration
- QA test plan for timer edge cases

**Dependencies:** Push notification infrastructure, Ad SDK integration

---

### 6.3 Implement Artifact Maintenance Degradation
**Difficulty:** 4/10  
**Priority:** Medium

**Scope:**
- Define degradation curves: how quickly artifacts lose "cleanliness" over time (e.g., 10% per day)
- Implement backend job to decrement cleanliness daily
- Update Venue UI to show degradation indicators (visual dirt, warning badges)
- Build "Maintain" action flow with Coin cost
- Send push notifications when artifacts drop below 50% cleanliness

**Deliverables:**
- Degradation algorithm spec
- Backend scheduled job
- Frontend Venue UI updates
- Push notification triggers

**Dependencies:** Venue feature complete, Push notification infrastructure

---

## 7. Monetization Implementation

### 7.1 Integrate Subscription (Trivia Tango Pro)
**Difficulty:** 7/10  
**Priority:** High

**Scope:**
- Configure subscription product in App Store Connect and Google Play Console (monthly, with free trial option)
- Integrate StoreKit 2 (iOS) and Google Play Billing Library (Android)
- Implement receipt validation on backend to prevent fraud
- Build paywall UI (`/paywall`) with benefits, pricing, and purchase flow
- Handle subscription lifecycle: purchase, renewal, cancellation, grace period, billing retry
- Add "Pro" badge to user profile and suppress ads for subscribers
- Implement "Restore Purchases" functionality

**Deliverables:**
- Subscription products configured in stores
- Native purchase flow (iOS + Android)
- Backend receipt validation
- Subscription status sync
- QA test plan covering all lifecycle states

**Dependencies:** App Store / Play Store accounts, Backend user system

---

### 7.2 Implement In-App Purchase (IAP) Store
**Difficulty:** 6/10  
**Priority:** High

**Scope:**
- Define IAP product catalog: Gem packs (e.g., 100, 500, 1200 Gems), Coin packs, Featured bundles
- Configure products in App Store Connect and Google Play Console
- Build Store UI (`/venue/store`) with product display, pricing, and purchase flow
- Implement backend fulfillment: credit currencies to user account upon successful purchase
- Handle purchase failures, pending transactions, and refunds
- Add analytics events: purchase initiated, purchase completed, revenue

**Deliverables:**
- IAP product catalog (stores + backend)
- Store UI implementation
- Purchase fulfillment logic
- Revenue analytics integration

**Dependencies:** Task 7.1 (shared billing infrastructure), Economy system

---

### 7.3 Integrate Rewarded Video Ads
**Difficulty:** 5/10  
**Priority:** Medium

**Scope:**
- Select and integrate ad mediation SDK (AdMob, ironSource, MAX)
- Define rewarded video placements: halve Mystery Box timer, extra hint, bonus Coins after game
- Implement reward fulfillment callbacks
- Configure ad frequency caps to avoid user fatigue
- Add fallback handling for no-fill scenarios
- Ensure Pro subscribers never see ads

**Deliverables:**
- Ad SDK integration
- 3 rewarded video placements
- Frequency cap configuration
- QA test plan

**Dependencies:** Task 7.1 (subscription check for ad suppression)

---

## 8. Analytics & KPI Tracking

### 8.1 Implement Analytics Event Schema
**Difficulty:** 5/10  
**Priority:** High

**Scope:**
- Define comprehensive event taxonomy covering:
  - Onboarding: signup_started, signup_completed, categories_selected
  - Gameplay: game_started, question_answered, game_completed, hint_used
  - Social: challenge_sent, challenge_accepted, friend_added, invite_sent
  - Economy: coins_earned, gems_earned, purchase_completed, mystery_box_opened
  - Retention: daily_mission_completed, streak_achieved, artifact_maintained
- Integrate analytics SDK (Amplitude, Mixpanel, or Firebase Analytics)
- Instrument all events in app code
- Validate data flow in analytics dashboard

**Deliverables:**
- Event taxonomy document
- Analytics SDK integration
- Event validation report

**Dependencies:** App codebase access

---

### 8.2 Build KPI Dashboards
**Difficulty:** 4/10  
**Priority:** High

**Scope:**
- Create dashboards for core KPIs defined in GTM Strategy:
  - **Acquisition:** CPI by channel, installs by source, K-factor (virality)
  - **Retention:** D1, D7, D30 retention curves, DAU/MAU ratio
  - **Monetization:** ARPDAU, conversion rate (free to paid), LTV by cohort, subscription churn
  - **Engagement:** games per DAU, average session length, mission completion rate
- Set up automated daily/weekly email reports to stakeholders
- Create alert thresholds for anomalies (e.g., D1 retention drops below 30%)

**Deliverables:**
- 4 KPI dashboards (Acquisition, Retention, Monetization, Engagement)
- Automated report configuration
- Alert threshold documentation

**Dependencies:** Task 8.1 (events flowing)

---

## 9. Soft Launch Execution

### 9.1 Prepare Soft Launch Markets
**Difficulty:** 3/10  
**Priority:** High

**Scope:**
- Select 2-3 Tier 2/3 English-speaking markets (e.g., Canada, Australia, Philippines) based on:
  - Representative user behavior
  - Lower CPI for testing
  - Manageable scale
- Configure App Store Connect and Google Play for geo-restricted release
- Prepare localized (or at least locale-appropriate) app store listings
- Coordinate with UA team on soft launch budget ($5K-$20K)

**Deliverables:**
- Soft launch market selection rationale
- Geo-restricted store configuration
- Soft launch UA budget plan

**Dependencies:** App ready for release, Tasks 4.1, 4.2

---

### 9.2 Execute Soft Launch & Monitor
**Difficulty:** 5/10  
**Priority:** High

**Scope:**
- Release app to soft launch markets
- Run UA campaigns at modest scale to acquire 5K-10K users
- Monitor server stability under real load
- Track baseline metrics: D1/D7/D30 retention, session length, crash rate, economy balance
- Collect qualitative feedback via in-app surveys or App Store reviews
- Identify and fix critical bugs before global launch

**Deliverables:**
- Soft launch release
- Daily monitoring reports (2-4 weeks)
- Bug fix prioritization list
- Soft launch retrospective document

**Dependencies:** Task 9.1, All retention/engagement systems live

---

### 9.3 Tune Economy Balance
**Difficulty:** 6/10  
**Priority:** High

**Scope:**
- Analyze soft launch data for economy health:
  - Are players earning Coins/Gems too fast or too slow?
  - Are Mystery Box drop rates satisfying?
  - Is the grind to upgrade Venue tiers reasonable?
- Adjust reward outputs, Store prices, and unlock costs based on data
- Re-run simulations and validate with a second cohort if needed
- Document final economy parameters

**Deliverables:**
- Economy analysis report
- Tuned economy parameters
- Updated game config

**Dependencies:** Task 9.2 (soft launch data)

---

## 10. Global Launch Execution

### 10.1 Scale UA Campaigns Globally
**Difficulty:** 6/10  
**Priority:** High

**Scope:**
- Expand UA campaigns to US, UK, and additional high-value markets
- Increase budget based on soft launch LTV:CAC learnings
- Launch new creative variants informed by soft launch performance
- Activate influencer partnerships (see Task 10.2)
- Coordinate launch PR (press releases, App Store feature requests)

**Deliverables:**
- Global UA campaign launch
- Budget scaling plan
- PR launch materials

**Dependencies:** Task 9.2 (soft launch learnings), Task 9.3 (economy tuned)

---

### 10.2 Activate Influencer Partnerships
**Difficulty:** 6/10  
**Priority:** Medium

**Scope:**
- Identify 10-20 influencers aligned with target personas:
  - Trivia/quiz YouTube channels
  - Mobile gaming TikTokers
  - "Edutainment" content creators
- Negotiate sponsorship deals or affiliate revenue shares
- Provide influencers with promo codes, deep links, and creative briefs
- Track influencer-driven installs and revenue via unique UTM parameters

**Deliverables:**
- Influencer shortlist and outreach tracker
- Sponsorship contracts
- Tracking link setup
- Campaign performance report

**Dependencies:** Task 3.1 (deep links for tracking)

---

### 10.3 Request App Store Feature
**Difficulty:** 3/10  
**Priority:** Medium

**Scope:**
- Submit App Store and Google Play feature request forms 6-8 weeks before global launch
- Highlight unique features (Venue collection, Story Cards, Glicko ratings)
- Provide high-resolution marketing assets and app preview video
- Follow up with Apple/Google developer relations contacts if available

**Deliverables:**
- Feature request submissions
- Marketing asset package for editorial teams

**Dependencies:** Task 4.2, Task 4.4 (screenshots and video)

---

## 11. Post-Launch LiveOps

### 11.1 Build LiveOps Content Pipeline
**Difficulty:** 5/10  
**Priority:** Medium

**Scope:**
- Establish process for creating and deploying new content:
  - New trivia questions (weekly refresh target)
  - Featured artifacts in Store (bi-weekly rotation)
  - Seasonal Mega Categories (e.g., "Spooky History" for Halloween)
- Build internal tools or CMS for content management
- Define QA process for new questions (accuracy, difficulty tagging)
- Create content calendar for first 6 months post-launch

**Deliverables:**
- Content pipeline process document
- CMS or internal tooling
- 6-month content calendar

**Dependencies:** Backend content management system

---

### 11.2 Implement Limited-Time Events System
**Difficulty:** 7/10  
**Priority:** Low

**Scope:**
- Design event framework supporting time-limited challenges, bonus rewards, and exclusive artifacts
- Build backend scheduling system for event start/end times
- Create event UI components (banners, countdown timers, event-specific screens)
- Launch first event within 4-6 weeks of global launch
- Measure event impact on DAU, session length, and revenue

**Deliverables:**
- Event system architecture
- First event design doc
- Event performance report

**Dependencies:** Global launch complete, Task 11.1

---

### 11.3 Establish Community & Support Channels
**Difficulty:** 3/10  
**Priority:** Medium

**Scope:**
- Set up official Discord server and/or subreddit for community engagement
- Create support email and FAQ/help center (Zendesk, Intercom, or similar)
- Define escalation paths for critical issues (crashes, billing problems)
- Monitor App Store reviews and respond to negative feedback within 24-48 hours
- Recruit community moderators for Discord/Reddit

**Deliverables:**
- Discord server / subreddit live
- Help center with 20+ FAQ articles
- Review response SLA documentation

**Dependencies:** None (can start pre-launch)

---

## Summary Table

| Task | Difficulty | Priority |
|------|------------|----------|
| 1.1 Conduct User Research Interviews | 4 | High |
| 1.2 Build Persona Documentation | 2 | Medium |
| 2.1 Finalize USP Messaging Framework | 5 | High |
| 2.2 Produce Video Ad Creatives | 7 | High |
| 2.3 Develop Playable Ad Unit | 8 | High |
| 2.4 Design Shareable Results Cards | 4 | Medium |
| 3.1 Implement Deep Link Infrastructure | 6 | High |
| 3.2 Build Referral Reward Fulfillment | 5 | High |
| 4.1 Implement App Store Listing Copy | 2 | High |
| 4.2 Produce App Store Screenshots | 5 | High |
| 4.3 Set Up A/B Screenshot Testing | 4 | Medium |
| 4.4 Create App Preview Video | 6 | Medium |
| 5.1 Set Up Ad Accounts & Tracking | 5 | High |
| 5.2 Define Audience Targeting Strategy | 4 | High |
| 5.3 Launch & Optimize UA Campaigns | 7 | High |
| 6.1 Implement Daily Missions System | 6 | High |
| 6.2 Build Mystery Box Timer & Notifications | 5 | High |
| 6.3 Implement Artifact Maintenance | 4 | Medium |
| 7.1 Integrate Subscription (Pro) | 7 | High |
| 7.2 Implement IAP Store | 6 | High |
| 7.3 Integrate Rewarded Video Ads | 5 | Medium |
| 8.1 Implement Analytics Event Schema | 5 | High |
| 8.2 Build KPI Dashboards | 4 | High |
| 9.1 Prepare Soft Launch Markets | 3 | High |
| 9.2 Execute Soft Launch & Monitor | 5 | High |
| 9.3 Tune Economy Balance | 6 | High |
| 10.1 Scale UA Campaigns Globally | 6 | High |
| 10.2 Activate Influencer Partnerships | 6 | Medium |
| 10.3 Request App Store Feature | 3 | Medium |
| 11.1 Build LiveOps Content Pipeline | 5 | Medium |
| 11.2 Implement Limited-Time Events | 7 | Low |
| 11.3 Establish Community & Support | 3 | Medium |

---

**Total Tasks:** 32  
**High Priority:** 20  
**Medium Priority:** 10  
**Low Priority:** 2  
**Average Difficulty:** 5.0
