# Plan B by we are the champions

> **When Plan A fails, Plan B saves the trip.**

**Track:** Lifestyle · Planning an Escape
**Team:** we are the champions
**Members:** Tan Poh Zhai, Lee Wai Loong, Yap Chun Hoong, Yap Shern Yu

---

## 1. Project Overview

### The Problem
Planning a group getaway with friends should be exciting, but in reality, it is usually stressful and chaotic:
- **Scattered Details:** Flight tickets get buried in email, date polls disappear in WhatsApp, places live in Instagram saves, and expenses get dumped into Splitwise days later.
- **Awkward Money Talks:** Friends have different budgets. Students or junior members often feel uncomfortable saying *"that restaurant is too expensive for me"*, so the group defaults to the highest spender's habits.
- **Sleep & Pace Clashes:** One friend wants to wake up at 6:00 AM to hike, while another refuses to get out of bed before 10:00 AM.
- **Fake AI Hallucinations:** Generic travel chatbots spit out generic tourist essays, often recommending closed shops, fake prices, or impossible routes.
- **Fragile Plans (The Domino Effect):** When heavy rain hits Penang or a ferry gets delayed, the entire day collapses. Nobody knows what to do next, and the trip leader ends up stressed out and blamed.

**Who This Is For:**
College students, young working adults, and small friend groups (2–6 people or solo travelers) who want a quick, easy, mobile-first way to plan weekend escapes without the drama.

---

### Why Existing Apps Fall Short
- **Wanderlog & TripIt:** Built for solo business travelers parsing corporate travel receipts. They feel clunky on mobile, lock basic collaboration behind paid paywalls, ignore individual budget limits, and have no way to quickly rescue a ruined afternoon.
- **Splitwise:** Strictly calculates debts *after* money is already spent. It has no connection to daily plans, so it cannot prevent groups from overspending upfront.
- **Google Maps Lists:** Great for saving bookmarks, but completely static. They cannot find common dates, respect travel pace, or help you adapt when unexpected delays happen.

---

### Our Solution: Plan B
**Plan B** is a phone-first Progressive Web App (PWA) and grounded AI assistant that helps friends plan, coordinate, and travel together smoothly:
1. **Painless Setup:** One friend creates a room and shares a 6-digit code or link. Friends join instantly on their phone browser without mandatory app store downloads.
2. **Upfront Alignment:** Everyone fills a quick 4-step form. Plan B automatically finds overlapping free dates and **locks the group budget to the lowest member's budget cap**, so no friend is ever priced out or embarrassed.
3. **Secret Wishlists:** Want to surprise a friend for their birthday? A private toggle lets you suggest a spot that stays hidden from the group, while the AI secretly routes it into the day.
4. **Real Maps, Zero Fake Venues:** Uses open-source OpenStreetMap with fast Photon autocomplete. Travelers search and tap real places onto their map canvas—the AI only arranges places members actually pinned. If zero places are added, the AI politely halts rather than inventing fake shops.
5. **The Plan B Rescue (Our Core Twist):** During the trip, if it pours rain or a venue is closed, tap **Delay**. Plan B recalculates **only that single disrupted hour**, suggesting 3 nearby dry indoor backups while keeping your booked flights and hotels 100% locked!
6. **Fair & Painless Bill Splitting:** Log expenses against daily activities with even or custom splits. The built-in debt equalizer figures out the fewest transfers needed to settle up.
7. **Dual-Channel AI (Private 1-on-1 & In-Room Group Chat):** Whether in **Plan Mode** (pre-trip) or **Trip Mode** (on the road), travelers can privately open the dedicated AI Agent (Screen 14) to ask 1-on-1 questions about their trip (*"Where is our hotel check-in?"*, *"How much budget do I have left?"*) without cluttering the group chat, or mention `@PlanB` in the shared team chat (Screen 20/21) when decisions need team visibility or consensus summaries.

#### The Core User Journey:
`Open Link / Code → Quick 4-Step Preferences → Align Dates & Lowest Budget → Pin Real Spots on Map → AI Drafts Schedule → Chat & @PlanB Consensus → Live Trip Mode (Done / Delay) → 1-Hour Plan B Rescue → Fair Bill Settlement.`

---

## 2. Ideation & Process

### 2.1 Ideas We Considered

| Approach | Verdict | Why We Chose or Dropped It |
| :--- | :--- | :--- |
| **A. Plan B Collaborative Workspace (Chosen)** | **Kept (Our Product)** | Replaces 5 fragmented apps with one clean mobile space. Automatically protects the lowest budget, uses only real map pins, and provides instant 1-hour replanning when plans fail. |
| **B. Vercel Serverless + Supabase Cloud (Chosen)** | **Kept (Tech Stack)** | Blazing-fast Nuxt 3 serverless rendering, secure PostgreSQL database for private wishlists, and built-in WebSockets for real-time group chat without running heavy servers. |
| **C. OpenStreetMap + Photon Search (Chosen)** | **Kept (Mapping)** | Fast search-as-you-type autocomplete and tap-to-pin without commercial Google Maps API fees, credit cards, or rate limits. |
| **D. In-Room Chat with @PlanB Concierge (Chosen)** | **Kept (Collaboration)** | Keeps communication inside the trip room. Friends can mention `@PlanB` to get instant answers or summarize 100+ chat messages into actionable agreements. |
| **E. All-in-One Booking Super-App with Live Flight/Hotel APIs** | **Dropped** | Airline and hotel booking APIs are notorious for rate limits, high fees, and checkout errors. Travelers book flights separately anyway; fake booking checkouts only create fragile demos. |
| **F. Free-Form Conversational Chatbot** | **Dropped** | Open-ended chatbots hallucinate closed restaurants, fake prices, and ignore group budget caps. |
| **G. Strict Public Nominatim Geocoder** | **Dropped** | Enforces a rigid 1-request-per-second limit that breaks smooth autocomplete typing. Replaced with Photon. |

---

### 2.2 System & User Flow Diagrams

All diagrams below illustrate how friends actually use Plan B in real life, from initial planning to live road trips.

#### 2.2.1 Why Group Trips Break Down (The Problem Story)
**The Breakdown:** Three common real-world friction points cause group trips to fall apart, leading to overspending, arguments, and ruined days.

```mermaid
flowchart TB
  subgraph CAUSES["Where Group Travel Goes Wrong"]
    C1["Scattered Across 5 Apps<br/>Tickets in email, dates in WhatsApp,<br/>places in Notes, bills in Splitwise"]
    C2["Unspoken Money & Sleep Conflicts<br/>Friends argue about budgets and 6 AM wake-ups<br/>in chaotic chat threads"]
    C3["Unreliable Travel AI Bots<br/>Generic chatbots invent fake shops,<br/>wrong prices, and impossible travel times"]
  end

  P["THE CORE BREAKDOWN<br/>No single shared truth for people, dates, budget, and daily plans"]

  subgraph EFFECTS["What Happens on the Trip"]
    E1["Endless Arguments & Overspending<br/>Group compromises upward; lowest-budget friends feel stressed"]
    E2["Awkward Post-Trip Money Chasing<br/>Complicated math days after returning home"]
    E3["Fake & Closed Places<br/>Itinerary looks good on paper, but shops are closed in real life"]
    E4["One Rainstorm Destroys the Day<br/>A single delay ruins the schedule because there is no quick Plan B"]
  end

  C1 --> P
  C2 --> P
  C3 --> P
  P --> E1
  P --> E2
  P --> E3
  P --> E4
```

---

#### 2.2.2 How Friends Experience Plan B (User Journey)
**The Flow:** How a group of friends (Alex, Jamie, Sam, Riley) plans, coordinates, and escapes together step-by-step.

```mermaid
flowchart LR
  Step1["1. Quick Invite<br/>Host creates room & shares<br/>6-digit code or link"] --> Step2["2. 4-Step Intake<br/>Friends enter dates, budget,<br/>pace, deal-breakers & secret spots"]

  Step2 --> Step3["3. Group Alignment<br/>App finds overlapping dates &<br/>locks budget to lowest cap (RM 450)"]

  Step3 --> Step4["4. Pin Real Places<br/>Friends search & tap favorite spots<br/>on OpenStreetMap canvas"]

  Step4 --> Step5["5. AI Drafts Days<br/>Gemini arranges real pinned stops<br/>into Day 1 & Day 2 by travel pace"]

  Step5 --> Step6["6. Chat & @PlanB<br/>Friends discuss in group chat;<br/>ask @PlanB to summarize agreements"]

  Step6 --> Step7["7. Live Trip Mode<br/>On the road: Next stop countdown,<br/>1-tap 'Done' or 'Delay'"]

  Step7 --> Step8["8. 1-Hour Plan B Rescue<br/>Rain hits? AI patches that 1 hour;<br/>hotels & flights stay 100% locked"]

  Step8 --> Step9["9. Fair Bill Split<br/>Log expenses anytime; equalizer<br/>settles debts in fewest transfers"]
```

---

#### 2.2.3 End-to-End Activity Flow
**How It Works Behind the Scenes:** Seamless collaboration between the Travelers, Plan B Engine, AI Assistant, and On-the-Ground Trip Mode.

```mermaid
flowchart TD
  subgraph THE_GANG["👥 The Travel Gang (Alex, Jamie, Sam, Riley)"]
    Start(["Start Planning"]) --> Join["Join via 6-Digit Code (688422)"]
    Join --> FillForm["Submit 4-Step Preferences<br/>(Dates, Budget, Pace, Deal-breakers)"]
    FillForm --> SecretChoice{"Want to plan a surprise?"}
    SecretChoice -->|Yes: Birthday / Date| HideSpot["Toggle 'Hide from group'<br/>(Kept secret from peer screens)"]
    SecretChoice -->|No: Regular Stop| PublicSpot["Save as Public Suggestion"]
    HideSpot --> PinSpots["Search & Pin Real Places on Map<br/>(Tek Sen, Kek Lok Si, Penang Hill)"]
    PublicSpot --> PinSpots
    PinSpots --> GroupChat["Coordinate in Group Chat<br/>(Share pins & discuss timing)"]
    GroupChat --> AskSummary["Ask '@PlanB summarize opinions'<br/>(AI extracts agreed stops & open items)"]
  end

  subgraph PLAN_B_APP["⚙️ Plan B System Engine"]
    FillForm --> CalcOverlap["Find Overlapping Free Dates (Oct 10-14)"]
    CalcOverlap --> LockBudget["Lock Group Budget Ceiling to Lowest Cap:<br/>min(2000, 800, 450) = RM 450/person"]
    LockBudget --> ReadyDashboard["Display Group Alignment Dashboard"]
    ReadyDashboard --> ConfirmCity{"Destination Confirmed<br/>by Group?"}
    ConfirmCity -->|Yes: Penang, Malaysia| CheckPins{"Any real places pinned?"}
  end

  subgraph AI_ASSISTANT["🤖 Trip-Scoped AI Assistant"]
    AskSummary --> SummarizeDebate["Summarize Chat Discussion:<br/>1. Agreed: Tek Sen dinner<br/>2. Desired: Relaxed Day 2 afternoon<br/>3. Action: One-tap apply to schedule"]
    SummarizeDebate --> PostDiff["Post Consensus Card into Group Chat"]
    PostDiff -.-> ConfirmCity

    CheckPins -->|No: 0 places| StopAlert["🛑 Guardrail Triggered:<br/>'No places pinned yet! Please add spots first.'<br/>(Refuses to invent fake places)"]
    StopAlert -.-> PinSpots

    CheckPins -->|Yes: Real pins exist| BuildDays["Draft Day-by-Day Itinerary:<br/>- Sequences real places by travel pace<br/>- Secretly routes surprise spot without leaking name<br/>- Keeps total estimated spend under RM 450 cap"]
  end

  subgraph ON_THE_ROAD["🚗 Live Trip & Fair Bill Splitting"]
    BuildDays --> GoTrip["Switch to Live Trip Mode on Travel Day"]
    GoTrip --> ActiveCard["NEXT STOP Card & Arrival Countdown<br/>(Kek Lok Si Temple · 09:00 AM)"]

    ActiveCard --> RoadStatus{"What happens on the road?"}
    RoadStatus -->|All Good| HitDone["Tap 'Done' (Arrived)"]
    RoadStatus -->|Disruption / Rain| HitDelay["Tap 'Delay' (+30m)<br/>Reason: Heavy rain at Air Itam"]

    HitDelay --> SaveStays["🔒 Protect Anchors:<br/>Booked hotels & return flights remain locked"]
    SaveStays --> QuickFix["AI: Suggest 3 dry indoor backup spots<br/>from your saved place pool"]
    QuickFix --> ActiveCard

    HitDone --> LogBill["Log Expense on Itinerary Stop<br/>(e.g. RM 120 Grab ride, split 4 ways)"]
    LogBill --> SettleDebt["Balance Equalizer:<br/>Shows exactly who pays who in 2 clean transfers"]
    SettleDebt --> FinishTrip(["Trip Complete & Fairly Settled!"])
  end
```

---

#### 2.2.4 Core Feature Flows (How Each Part Works)

##### 1. Finding Common Ground (Date Overlap & Lowest Budget Ceiling)
**The Problem Solved:** Friends talk about dates and budgets in circles. Plan B automatically finds when everyone is free and locks the budget to the lowest spender so nobody feels excluded.

```mermaid
flowchart TD
  F1["Alex (Host)<br/>Dates: Oct 10-15 · Cap: RM 2000 · Pace: Easy"] --> MatchEngine["Plan B Alignment Engine"]
  F2["Jamie<br/>Dates: Oct 10-15 · Cap: RM 800 · Pace: Balanced"] --> MatchEngine
  F3["Riley<br/>Dates: Oct 10-14 · Cap: RM 450 · Pace: Relaxed"] --> MatchEngine

  MatchEngine --> Win["Find Common Free Window:<br/>Oct 10 - 14 (5 Days)"]
  MatchEngine --> Cap["Lock Budget Ceiling to Lowest Cap:<br/>RM 450 / person (Protects Riley from overspending!)"]
  MatchEngine --> Rules["Aggregate All Deal-Breakers:<br/>No 4:00 AM wake-ups · Vegetarian meals for Jamie"]

  Win --> SummaryCard["Render Group Alignment Summary<br/>(3/4 Ready · Overlap: Oct 10-14 · Cap: RM 450)"]
  Cap --> SummaryCard
  Rules --> SummaryCard

  SummaryCard --> ConfirmDest{"Destination Confirmed?"}
  ConfirmDest -->|Yes: Penang, Malaysia| UnlockPlan["Unlock Itinerary & Map Planning ✓"]
```

---

##### 2. Keeping a Secret Surprise (Hidden Wishlists)
**The Problem Solved:** Planning a birthday cake pickup or surprise dinner usually gets leaked in group chats. Plan B lets a member submit a private spot that peers cannot see, while the AI secretly routes it into the day.

```mermaid
flowchart TD
  Alex["Traveler (Alex)"] --> Propose["Proposes Secret Spot: 'Kyoto Sunset Villa'"]
  Propose --> Switch["Toggle: Hide from group (Surprise)"]

  Switch --> SharedView["Shared Group View (Jamie, Sam, Riley):<br/>Map displays generic 'Surprise Activity'<br/>Real coordinates and venue name are hidden!"]
  Switch --> PrivateView["Alex's Private View:<br/>Shows 'Private Wishlist' indicator"]

  Switch --> ServerAI["Trip-Scoped AI Server Route"]
  ServerAI --> SafeInstruction["System Instruction:<br/>'Factor this location into the travel route,<br/>but NEVER reveal its real name in public outputs.'"]
  SafeInstruction --> DraftRoute["Generated Day Plan:<br/>Group gets a seamless route; surprise stays 100% secret!"]
```

---

##### 3. Pinning Real Places on OpenStreetMap
**The Problem Solved:** No reliance on expensive commercial Google Maps keys or broken booking engines. Search real spots in seconds with Photon autocomplete or tap anywhere on the map to pin custom coordinates.

```mermaid
flowchart TD
  HowToAdd{"How Traveler Adds a Stop"}

  HowToAdd -->|Search Bar| TypeSearch["Type: 'Tek Sen Restaurant'"]
  TypeSearch --> AutoList["Instant Photon Suggestions<br/>(Real Penang street addresses via OSM)"]
  AutoList --> PickOne["Traveler selects Tek Sen"]

  HowToAdd -->|Tap Map Canvas| TapMap["Tap anywhere on Leaflet map canvas"]
  TapMap --> CustomPin["Drops custom pin (e.g. 'Scenic Beach Viewpoint')"]

  PickOne --> SavePool["Save to Trip Place Pool<br/>(Saved pins become approved candidates for the trip)"]
  CustomPin --> SavePool

  SavePool --> RenderPin["Shows Numbered Pin on Map<br/>(Pin 1, Pin 2, Pin 3...)"]
  RenderPin --> AddDrawer["Bottom Sheet: '+ Add to Itinerary Pool'"]
```

---

##### 4. AI Drafting the Schedule (Strictly Grounded, No Fake Shops)
**The Problem Solved:** Travel bots often invent restaurants that closed three years ago. Plan B's AI is strictly forbidden from inventing places—it only sequences the spots your group actually pinned, and halts if zero places exist.

```mermaid
flowchart TD
  ClickGen["Traveler taps: 'Generate Itinerary'"] --> CheckCount{"How many places in trip pool?"}

  CheckCount -->|0 Places Added| HaltEngine["🛑 Safety Guardrail Triggered:<br/>Cannot generate schedule: 0 places in pool.<br/>Please search or tap the map to add places first.<br/>(Zero fake restaurants or invented prices!)"]

  CheckCount -->|"Places Exist (Tek Sen, Kek Lok Si...)"| PrepareData["Feed Trip Parameters to AI:<br/>- Pinned Places: Kek Lok Si, Tek Sen, Penang Hill<br/>- Lowest Budget Cap: RM 450<br/>- Group Pace: Easy (No rushing)"]

  PrepareData --> GeminiRun["Gemini 2.5 Flash:<br/>Calculates logical travel sequence & estimated times"]
  GeminiRun --> VerifyOutput["Verify All Venue IDs Match Real Pinned Spots"]
  VerifyOutput --> RenderSchedule["Render Day-by-Day Itinerary Cards<br/>(Day 1: Kek Lok Si 09:00, Lunch 12:30; Day 2: Penang Hill 10:00)"]
```

---

##### 5. The Plan B Rescue (Sudden Rain or Delay in Trip Mode)
**The Problem Solved (Our Core Innovation):** When a storm hits or an attraction is closed, traditional apps make you redo the entire 3-day schedule. Plan B patches **only that specific 1-hour slot**, keeping your booked hotel and flight times 100% locked!

```mermaid
flowchart TD
  LiveTrip["Live Trip Mode in Penang<br/>NEXT STOP: Kek Lok Si Temple (09:00 AM)"] --> Storm{"Disruption Happens!<br/>Heavy rainstorm at Air Itam"}

  Storm --> DelayTap["Member taps: 'Delay (+30m)' or 'Skip'"]
  DelayTap --> LockRule["🔒 Protect Key Anchors:<br/>- Hotel Check-in: LOCKED (15:00)<br/>- Return Flight: LOCKED (19:00)<br/>(Never moved or rescheduled!)"]

  LockRule --> SearchIndoor["AI checks unused saved pins for nearby indoor spots"]
  SearchIndoor --> GiveOptions["Present 3 Dry Indoor Plan B Backups:<br/>1. Penang State Museum (Indoor heritage)<br/>2. ChinaHouse Cafe (Indoor cafe & art)<br/>3. Cheong Fatt Tze Blue Mansion (Indoor tour)"]

  GiveOptions --> PickFix["Group picks ChinaHouse Cafe"]
  PickFix --> PatchSlot["Patches ONLY the 09:00-10:30 slot in database<br/>Rest of the trip stays perfectly intact!"]
```

---

##### 6. Fair & Painless Bill Splitting (Zero Awkward Money Talks)
**The Problem Solved:** Splitwise only tracks money days later when everyone has already overspent. Plan B logs receipts directly against itinerary stops and minimizes the number of transfers so friends don't make 10 tiny payments.

```mermaid
flowchart TD
  Receipt1["Alex pays RM 240 for Rooftop Drinks<br/>Custom Split: Riley drank water = RM 0"] --> MoneyLedger["Trip Ledger: Total Spent RM 368.50"]
  Receipt2["Jamie pays RM 128.50 for Grab rides<br/>Even Split: RM 32.12 each"] --> MoneyLedger

  MoneyLedger --> BalanceMath["Calculate Fair Share for Each Person"]
  BalanceMath --> S1["Alex: Gets RM 147.87"]
  BalanceMath --> S2["Jamie: Gets RM 36.37"]
  BalanceMath --> S3["Sam: Owes RM 92.12"]
  BalanceMath --> S4["Riley: Owes RM 92.12"]

  S1 --> SmartSettlement["Smart Debt Simplifier<br/>(Calculates the fewest direct payments)"]
  S2 --> SmartSettlement
  S3 --> SmartSettlement
  S4 --> SmartSettlement

  SmartSettlement --> Pay1["1. Sam sends Alex RM 92.12"]
  SmartSettlement --> Pay2["2. Riley sends Alex RM 55.75"]
  SmartSettlement --> Pay3["3. Riley sends Jamie RM 36.37"]

  Pay1 --> CardUI["Display Balance Equalizer Cards<br/>Tap Mark Paid button to settle in 1 second!"]
  Pay2 --> CardUI
  Pay3 --> CardUI
```

---

##### 7. In-Room Group Chat with @PlanB Concierge & Consensus Summarizer
**The Problem Solved:** 100+ WhatsApp messages make trip leaders go crazy trying to figure out what everyone agreed on. Inside Plan B's chat room, friends can mention `@PlanB` to get instant answers or summarize long debates into clean decisions.

```mermaid
flowchart TD
  subgraph THE_CHAT["💬 In-Room Group Chat (Screen 20 & 21)"]
    M1["Jamie: 'I pinned Tek Sen on the map, their food is great!'"]
    M2["Sam: 'I'm exhausted, can we keep Day 2 afternoon relaxed for coffee?'"]
    M3["Riley: 'As long as total spend stays under RM 450 I'm good!'"]
    AskAI["Alex: '@PlanB summarize what we agreed on'"]
  end

  subgraph THE_CONCIERGE["🤖 In-Room AI Concierge (@PlanB)"]
    DetectQuery{"Did someone mention @PlanB?"}
    ParseChat["Read Chat History & Member Preferences:<br/>- Agreed stop: Tek Sen dinner<br/>- Pacing desire: Sam's relaxed Day 2 afternoon<br/>- Budget check: Confirmed within Riley's RM 450 cap"]
    CreateDiff["Generate Consensus Summary Card with<br/>Apply to Itinerary action button"]
  end

  M1 --> DetectQuery
  M2 --> DetectQuery
  M3 --> DetectQuery
  AskAI --> DetectQuery

  DetectQuery -->|Yes: @PlanB| ParseChat
  ParseChat --> CreateDiff
  CreateDiff --> PostFeed["Broadcast Summary Card directly into Chat Feed!"]
  PostFeed --> AllSee["All 4 friends see the exact same agreement;<br/>1 tap updates the schedule for everyone!"]
```

---

#### 2.2.5 Screen-by-Screen Walkthrough (Mapped to 21 Figma Screens)
**The Complete App Flow:** Seamless progression from onboarding to live travel.

```mermaid
flowchart TD
  S01["01 Splash<br/>When Plan A fails..."] --> S02["02 Login / Register<br/>Email or Google/Apple"]
  S02 --> S03["03 Trips (Empty State)<br/>Create Trip / Join with Code"]
  S03 --> S04["04 Trip Home: Align<br/>Alex (Host), Jamie, Sam, Riley"]
  S03 --> S17["17 Invite<br/>6-Digit Code 688422 & Link"]
  S04 --> S05_08["05-08 Preferences 1-4<br/>Destination, Dates, Budget, Interests"]
  S05_08 --> S09["09 Align Filled<br/>Dates Overlap & RM 450 Cap Locked"]
  S09 --> DestCheck{"Confirm Destination?<br/>Penang, Malaysia"}
  DestCheck -->|No| S04
  DestCheck -->|Yes| S10["10 Trip Home: Plan<br/>4 Entry Cards"]
  S10 --> S11["11 Map<br/>Search & Pin Real Places"]
  S10 --> S20_21["20-21 Group Chat<br/>Discuss, @PlanB Consensus & Pins"]
  S20_21 -.->|Apply Consensus Diff| S14
  S11 --> PlaceCheck{"Places Pinned > 0?"}
  PlaceCheck -->|No: Halt| S11
  PlaceCheck -->|Yes| S14["14 AI Agent<br/>Arranges Real Pins by Pace"]
  S14 --> S12_13["12-13 Itinerary<br/>Day 1 & Day 2 Real Stops"]
  S12_13 --> S18["18 Trip Mode<br/>NEXT STOP: Kek Lok Si (09:00 AM)"]
  S18 --> DisCheck{"Rain or Delay?"}
  DisCheck -->|Yes: Delay| S19["19 Mid-Trip Replan<br/>1-Hour Fix; Stays Locked"]
  S19 --> S18
  DisCheck -->|No: Settle up| S15["15-15b Money<br/>Total Spent RM 368.50 & Splits"]
  S03 --> S16["16 Profile: You<br/>Trip Count & Settings"]
```

> **Persistent Bottom Navigation Tabs (Always Accessible):**
> `Trips · Map · Plan · Money · Chat · You`

---

#### 2.2.6 How Our Architecture Evolved
**Why We Built It This Way:** We intentionally threw out fragile components (commercial booking APIs, strict geocoders) to keep the app 100% reliable and realistic.

```mermaid
flowchart LR
  V1["V1: Booking Super-App<br/>(Live flight/hotel checkout APIs)<br/>❌ Failed: Broken checkouts & rate limits"] --> V2["V2: Strict Public Geocoder<br/>(Public Nominatim 1 req/s)<br/>❌ Failed: Slow typing & wrong countries"]
  V2 --> V3["V3 FINAL: Plan B PWA<br/>Nuxt 3 + Supabase + OSM + Photon + Gemini<br/>✅ Success: Fast, grounded, zero fake data!"]
```

---

#### 2.2.7 Why Plan B Beats Traditional Alternatives

```mermaid
flowchart TB
  Q["How should friends plan a trip and handle unexpected delays?"]
  Q --> A["A. Heavy Booking Apps (Wanderlog / TripIt)<br/>❌ Clunky mobile UI, paid paywalls, no budget protections, no 1-hour rain rescue"]
  Q --> B["B. Group Chat + Splitwise<br/>❌ 100+ unread messages, money tracked days too late, no visual map or schedule"]
  Q --> C["C. Generic Travel AI Bots<br/>❌ Hallucinates fake shops, ignores who has less money, no real-time adaptation"]
  Q --> D["D. Plan B Grounded Workspace (Our Solution)<br/>✅ Locks budget to lowest spender, pins real places, rescues broken hours in 1 tap!"]
```

---

### 2.3 Mentor Consultation

*Notes and key feedback from mentor consultations will be recorded here during development.*

---

## 3. Design & Prototype

### Key Screens & User Interactions

Plan B is designed as a phone-first Progressive Web App (PWA) with a persistent 6-tab bottom navigation bar (`Trips · Map · Plan · Money · Chat · You`), allowing travelers to navigate with one hand on the move.

1. **Screen 01 & 02: Splash & Authentication**
   Clean brand opening with our slogan (*"When Plan A fails, Plan B saves the trip"*). Quick sign-up and login with email or Google/Apple accounts.

2. **Screen 03: Trips Home (Empty State)**
   Warm greeting with a `+ New Trip` button and a direct `Join with code` link.

3. **Screen 04: Trip Home (Align Mode)**
   Shows trip name (*"Penang with the gang"*), 4 members (Alex Host, Jamie, Sam, Riley), and an `+ Invite` button. Segmented control toggles between **Align** and **Plan** modes.

4. **Screen 05–08: 4-Step Preference Intake Wizard**
   - **Step 1 (Destination):** Suggest destinations with a `Hide from group` toggle for secret surprises.
   - **Step 2 (Dates):** Enter available date windows (`10/10/2026 to 10/15/2026`).
   - **Step 3 (Budget & Pace):** Set personal spending limit and pace (`Slow`, `Easy`, or `Fast`).
   - **Step 4 (Interests & Deal-Breakers):** Pick interest tags (`Culture`, `Food`, `Nature`, `Shopping`) and hard deal-breakers (`No 4am starts`).

5. **Screen 09 & 10: Alignment Summary & Trip Home (Plan Mode)**
   Displays computed overlapping dates, the group budget ceiling (`RM 450 / person`), and completion progress (`1/4 prefs in`). Locking the destination unlocks the 4 Plan cards: `Itinerary`, `Map`, `AI Agent`, and `Budget`.

6. **Screen 11: Grounded Dual-Mode Map**
   Interactive OpenStreetMap view. Search spots via fast Photon autocomplete or tap directly on the map to drop custom pins. Saved pins become approved stops for the trip pool—zero fake shops.

7. **Screen 12 & 13: Grounded Day-by-Day Itinerary**
   Timeline of sequenced activities (Day 1: `Kek Lok Si Temple 09:00`, `Char Koay Teow lunch 12:30`; Day 2: `Penang Hill 10:00`). Booked accommodations and flights display a green `🔒 Locked Stay` badge.

8. **Screen 14: Trip-Scoped AI Agent (Private 1-on-1 Oracle & Itinerary Controller)**  
   Accessible in **both Plan Mode (pre-trip) and Live Trip Mode (on the road)**. A dedicated 1-on-1 assistant strictly scoped to this trip's live parameters (`3 pins · 1/4 prefs · Budget cap: RM 450`). Travelers can privately ask trip-specific questions (*"What time do we leave tomorrow?"*, *"Who owes money right now?"*, *"Is lunch vegetarian-friendly for Jamie?"*) without broadcasting to the group chat, as well as tap quick chips (`Relax pace`, `Swap Day 2`) to adjust the schedule.

9. **Screen 15 & 15b: Money & Balance Equalizer**
   Itemized shared expenses showing `TOTAL SPENT RM 368.50` (`RM 92.13 / person`). Supports **Even Split** (Grab rides) and **Custom Split** (Rooftop drinks, where Riley is set to RM 0). The Balance Equalizer calculates the fewest transfers to settle up with one-tap `Mark Paid`.

10. **Screen 16 & 17: Profile (You) & Invite**
    Displays traveler profile and trip stats. The Invite screen provides a 6-digit code (`688422`) and direct link (`planb.app/join/688422`) for zero-friction guest joining.

11. **Screen 18 & 19: Live Trip Mode & Mid-Trip Replan**
    On the day of travel, Trip Mode highlights the active `NEXT STOP` card with `Done`, `Delay`, and `Skip` buttons. When rain or delays happen, the **Mid-Trip Replan** modal recalculates *only* that disrupted 1-hour slot with nearby dry indoor options, keeping all booked flights and hotels untouched.

12. **Screen 20 & 21: Real-Time Team Group Chat with In-Room AI Concierge**
    An in-room chat (via the `Chat` tab) where friends share map pins and coordinate plans. Travelers can mention `@PlanB` to ask questions or prompt *"@PlanB summarize our opinions"* to turn long debates into a clean agreement card with a one-tap update button.

---

## 4. What Makes It Different

| Feature | Traditional Travel Apps (Wanderlog, TripIt) | Group Chat + Splitwise | Generic Travel AI Bots | **Plan B (Our Solution)** |
| :--- | :--- | :--- | :--- | :--- |
| **Itinerary Creation** | Manual email parsing or static lists | None; messy text in chat notes | Hallucinates fake shops & wrong prices | **Sequenced strictly from real member-added pins** |
| **Budget Protection** | Passive cost display; ignores caps | Retroactive math after overspending | Ignores budgets or invents costs | **Locks group ceiling upfront to lowest member cap** |
| **Surprise / Private Stops** | Non-existent; everything is public | Leaked immediately in group chat | N/A | **Hidden destination toggle (server-side AI shielding)** |
| **Mid-Trip Disruption Fix** | Manual multi-day rescheduling | Panic in chat; manual reshuffling | Re-generates entire trip from scratch | **Single-slot 1-hour replan (hotels & flights stay locked)** |
| **Expense Settlement** | Requires paid tier or separate app | Disconnected from daily agenda | None | **In-room debt equalizer tied directly to daily stops** |
| **Team Communication** | External (WhatsApp / Telegram) | Fragmented across chat history | Single-player chatbot session | **In-room group chat with @PlanB concierge & consensus summary** |

### Key Product Highlights:
1. **The Lowest-Cap Ceiling (`min-cap`):** Group trips overspend because high spenders dominate. Plan B locks the ceiling to the lowest member budget (e.g. RM 450), keeping travel accessible to everyone.
2. **Hidden Wishlist Shield:** Propose a birthday surprise without spoiling it. The AI routes it secretly without naming it to peers.
3. **The Constrained Gemini Guardrail:** The AI is an optimizer, not an unconstrained writer. If zero places are pinned, it halts immediately instead of inventing fake shops.
4. **Single-Slot Replanning (The Core Plan B):** When a rainstorm hits, don't rewrite 3 days. Plan B patches only that disrupted hour, protecting all booked hotels and flights.
5. **Dual-Channel AI Assistance (Private 1-on-1 + In-Room Concierge):** Whether in **Plan Mode** (pre-trip) or **Trip Mode** (on the road), travelers have two dedicated ways to interact with AI:
   - **Private 1-on-1 Agent (Screen 14):** Open anytime to privately ask trip questions (hotel check-in, personal debts, dietary options) without cluttering the group chat.
   - **In-Room Concierge (`@PlanB` in Screen 20/21):** Mentioned directly in team chat when decisions require group visibility, agreement, and one-tap schedule updates.

---

## 5. Technical Architecture & Feasibility

### Tech Stack

| Layer | Technology Chosen | Why We Chose It | Constraints & How We Address Them |
| :--- | :--- | :--- | :--- |
| **Frontend Framework** | **Nuxt 3 (Vue 3, TypeScript)** | Modern Vue ecosystem, native `<ClientOnly>` wrappers for Leaflet maps, and first-class PWA support via `@vite-pwa/nuxt`. | SSR hydration mismatch with browser-only Leaflet maps. Resolved using Nuxt's `<ClientOnly>` component boundary. |
| **Backend / Serverless** | **Nitro Engine (Nuxt 3 Server Handlers)** | Fast, zero-config serverless route handlers deployed natively to Vercel with near-zero cold starts. | Stateless execution. Handled cleanly via client webhooks and Supabase Realtime subscriptions. |
| **Database & ORM** | **Supabase (PostgreSQL) + Drizzle ORM** | Drizzle offers pure TypeScript schemas with zero heavy binaries. Supabase provides managed PostgreSQL, Row-Level Security (RLS), and Realtime WebSockets. | Connection limits in serverless. Addressed by using Supabase's transaction connection pooler on port 6543. |
| **Realtime Services** | **Supabase Realtime** | Native PostgreSQL change-data-capture WebSockets for instant chat messaging and live disruption alerts. | Free-tier connection limits. Handled easily by scoping channels per trip room ID. |
| **AI Engine** | **Google Gemini 2.5 Flash** | High context efficiency, rapid structured JSON outputs, and strict adherence to system instructions. | Potential LLM hallucination. Prevented by strict allow-list schema validation rejecting unknown place IDs. |
| **Mapping & Geocoding** | **Leaflet + OpenStreetMap + Photon** | Open-source mapping stack. Leaflet provides smooth map rendering; Photon provides fast fuzzy search autocomplete without API keys or fees. | Coverage edge cases. Mitigated by allowing direct map canvas tapping for precise custom coordinates anywhere. |
| **Hosting & CI/CD** | **Vercel + Supabase Cloud** | Global edge network with zero server maintenance, automated GitHub deployments, and production HTTPS. | Serverless function timeout limits (10s on hobby). Gemini and Drizzle queries execute within 1.5–2.5s. |

---

### System Architecture Diagram

```mermaid
flowchart TD
  subgraph CLIENT["Client Layer: Phone-First PWA (390px Mobile Viewport)"]
    UI["Nuxt 3 PWA UI<br/>(Vue 3 · Pinia · Tailwind CSS)"]
    MapClient["Leaflet OSM Engine<br/>(<ClientOnly> Interactive Canvas)"]
    RTClient["Supabase Realtime Client<br/>(Live Chat & Replan Alerts)"]
  end

  subgraph SERVER["Serverless Backend Layer (Nitro Engine on Vercel)"]
    AuthRoute["/api/auth<br/>Session & Identity"]
    AlignRoute["/api/trips/align<br/>Date Overlap & Lowest Cap Math"]
    GeoRoute["Photon Geocoding Proxy<br/>(Fuzzy Search Autocomplete)"]
    GeminiRoute["/api/gemini/schedule<br/>Allow-List Grounded Planner"]
    ReplanRoute["/api/gemini/replan<br/>1-Hour Surgical Patch Engine"]
    DebtRoute["/api/money/settle<br/>Debt Graph Simplifier"]
  end

  subgraph STORAGE["Data & Security Layer (Supabase Cloud)"]
    DB[(PostgreSQL Database)]
    RLS["Row-Level Security (RLS)<br/>Private Destination Isolation"]
    RTServer["Supabase Realtime Engine<br/>(Postgres Change WebSockets)"]
  end

  subgraph EXTERNAL["External AI & Open-Source Geocoder"]
    GeminiAPI["Google Gemini API<br/>(Structured JSON Mode)"]
    PhotonAPI["Photon Geocoder<br/>(OpenStreetMap by Komoot)"]
  end

  UI --> AuthRoute
  UI --> AlignRoute
  UI --> GeoRoute
  UI --> GeminiRoute
  UI --> ReplanRoute
  UI --> DebtRoute
  UI --> MapClient
  UI <--> RTClient

  GeoRoute --> PhotonAPI
  GeminiRoute --> GeminiAPI
  ReplanRoute --> GeminiAPI

  GeminiRoute -.->|Strict ID Verification| DB
  ReplanRoute -.->|Preserve Locked Stays| DB

  AuthRoute --> DB
  AlignRoute --> DB
  DebtRoute --> DB
  DB --> RLS
  DB <--> RTServer
  RTServer <--> RTClient
```

---

### Build Plan & Scope

#### In-Scope (What We Build):
- [x] Clean phone-first authentication with personal travel defaults (pace, dietary restrictions).
- [x] Trip room lifecycle with instant 6-digit code sharing (`688422`) and direct link join.
- [x] 4-step preference submission wizard with the "Hidden Destination" privacy toggle.
- [x] Automated alignment engine computing date intersections and lowest-budget ceiling (`min-cap`).
- [x] OpenStreetMap with Photon fuzzy autocomplete search and direct tap-to-pin coordinate saving.
- [x] Server-side Gemini itinerary sequencing restricted strictly to member-added place IDs, with a 0-place halt guardrail.
- [x] Plan Mode (preparation) and Trip Mode (live next-stop countdown) state machine.
- [x] Single-slot replan modal replacing only the disrupted hour while preserving locked flights and hotel stays.
- [x] Itinerary-tied expense logging with automated debt graph calculation (Balance Equalizer).
- [x] Real-time in-room team group chat powered by Supabase Realtime with place card sharing, live disruption alerts, and `@PlanB` consensus summarizer.
- [x] Mobile-first PWA interface with persistent 6-tab navigation (`Trips · Map · Plan · Money · Chat · You`).

#### Out-of-Scope (Explicit Anti-Features):
- ❌ **Commercial Flight & Hotel Booking APIs:** No live ticket checkouts or dynamic inventory scrapers. Bookings are represented as user-entered, locked anchor items.
- ❌ **Nearby Auto-Scrapers:** The app never pulls unverified external restaurant lists; it schedules only venues intentionally added by travelers.
- ❌ **Dynamic Currency Fare Speculation:** Expense splitting is based on actual logged receipts, not speculative live foreign exchange scrapers.
