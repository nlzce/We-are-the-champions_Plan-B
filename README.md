# Plan B by We are the champions

> **When Plan A fails, Plan B saves the trip.**

**Track:** Lifestyle · Planning an Escape
**Team:** We are the champions
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

```mermaid
flowchart LR
  subgraph OLD["❌ The Old Way (Chaotic & Stressful)"]
    direction TB
    O1["Scattered Across 5 Apps<br/>(WhatsApp chats, Notes, Splitwise)"]
    O2["Awkward Budget Talks<br/>(Lower-budget friends forced to overspend)"]
    O3["Hallucinating AI Bots<br/>(Inventing closed shops & fake prices)"]
    O4["1 Rainstorm Ruins the Day<br/>(Entire 3-day schedule collapses)"]
    O1 --> O2 --> O3 --> O4
  end

  subgraph NEW["✅ The Plan B Way (Effortless & Fun)"]
    direction TB
    N1["1 Shared Mobile Space<br/>(Scan dynamic QR code to join in 2s)"]
    N2["Automatic Budget Protection<br/>(Ceiling strictly locked to lowest spender)"]
    N3["Real OpenStreetMap Pins<br/>(AI only sequences spots you actually added)"]
    N4["1-Hour Surgical Rescue<br/>(Patches rain delay, hotels/flights stay locked)"]
    N1 --> N2 --> N3 --> N4
  end

  OLD -.->|"Solved by Plan B"| NEW
```

**Plan B** is a phone-first Progressive Web App (PWA) powered by a **Cloudflare-native Multi-Agent System (Mastra Framework)**. It helps friends plan, coordinate, and travel together smoothly without stress or fake data:
1. **Easy 2-Second Join:** One friend creates a room and shows a **QR code** or shares a link. Friends scan the QR code with their phone camera to jump right into the room—no app store downloads needed.
2. **No Boring Forms:** You never have to fill out long, rigid travel forms. Just chat with your Personal AI Agent, send a voice memo, or drop screenshots from Xiaohongshu or Instagram. The AI automatically grabs your dates, budget limits, travel pace, food allergies, and secret wishlists in seconds.
3. **Your Own Personal AI Helper:** Every friend gets their own dedicated **Personal AI Agent** (built on **Mastra**). It works for you—learning your habits, protecting your wallet, and keeping your surprise birthday plans hidden from your friends.
4. **The Admin Agent (The Group Arbiter):** While your personal agent looks out for you, one central **Admin Agent** manages the trip room. It checks everyone's availability, finds dates when all friends are free, and caps the group budget to match the friend with the least money (`min-cap`). This ensures nobody feels broke, left out, or pressured to overspend.
5. **Real Map Spots, Zero Fake Shops:** Uses open-source OpenStreetMap with fast Photon search. The AI only arranges places that you and your friends actually pinned on the map. If you haven't pinned any places yet, the AI stops immediately instead of making up fake cafes or closed restaurants.
6. **The Plan B Rescue (Our Core Twist):** If it pours rain or an attraction is closed during the trip, just tap **Delay**. The AI fixes **only that ruined 1 hour** by finding 3 dry indoor backup spots nearby—while keeping your booked flights and hotels 100% untouched!
7. **Easy Bill Splitting Without Math:** Never fill out manual accounting forms. Just tell your AI agent what you spent via voice note, text, or a photo of your receipt. The Money tab directly shows simple cards of who owes whom, with a 1-tap "Mark Paid" button.
8. **Two Ways to Talk to AI:** Talk privately with your Personal Agent without spamming the group chat, or type `@PlanB` in the group room to get quick answers and turn long debates into a 1-tap agreement card.
9. **Fast Cloudflare Speed & Flexible AI:** Hosted entirely on **Cloudflare** (Pages, Workers, and **Cloudflare D1** serverless SQL database) so everything loads instantly on your phone. You can run it with 100% free **Cloudflare Workers AI** or switch to **Google Gemini 2.5 Flash** for heavy photo scanning.

#### The Core User Journey:
`Scan QR Code / Open Link → Chat with Personal Agent (No Forms) → Personal Agents Deliberate → Admin Agent Sets Lowest Budget Cap → Pin Real Spots on Map → AI Sequences Schedule → Live Trip Mode (Done / Delay) → 1-Hour Plan B Rescue → Easy Expense Voice Logging & 1-Tap Bill Settlement.`

---

### System Topology Mindmap

A clear bird's-eye view of how Plan B works across its 8 main building blocks:

```mermaid
flowchart LR
  Root(["<b>🗺️ Plan B PWA</b><br/><i>When Plan A fails, Plan B saves the trip</i>"])

  Root --> U["<b>1. Friends & Rooms</b><br/>• Students & friend groups<br/>• Scan QR code or tap link to join<br/>• Works right in phone browser (PWA)"]
  Root --> P["<b>2. Easy Intake (No Forms)</b><br/>• Drop screenshots (Xiaohongshu/IG)<br/>• Send voice notes & quick chats<br/>• AI turns them into trip cards"]
  Root --> A["<b>3. Mastra Multi-Agent Engine</b><br/>• Personal AI helper for each friend<br/>• Admin Agent arbiter for group decisions<br/>• Automated safety test checks"]
  Root --> M["<b>4. Real Map Engine</b><br/>• OpenStreetMap + Leaflet canvas<br/>• Fast search autocomplete<br/>• Only real pins, stops if 0 places"]
  Root --> D["<b>5. Planning & Live Trip Mode</b><br/>• Plan Mode: Pick dates & arrange stops<br/>• Trip Mode: Next stop countdown<br/>• 1-hour quick fix if it rains"]
  Root --> B["<b>6. Fair Money Split</b><br/>• Auto-logs expenses from receipts<br/>• Custom splits (e.g. non-drinkers pay $0)<br/>• Shows who owes who with 1-tap Pay"]
  Root --> S["<b>7. Private & Group AI Chat</b><br/>• Private 1-on-1 helper<br/>• Group chat concierge @PlanB<br/>• Mobile-first 6-tab PWA"]
  Root --> C["<b>8. Cloudflare Edge Stack</b><br/>• Cloudflare Pages & Workers hosting<br/>• Cloudflare D1 SQL database<br/>• Flexible AI: Workers AI OR Gemini"]
```

#### Mindmap Pillar Summary

```text
Plan B System Topology
├── 1. Friends & Rooms: Students & friends | Solo or group | QR code & link join | Phone-first PWA
├── 2. Easy Intake: Screenshots (Xiaohongshu/IG/Maps) | Voice memos & text | Zero-shot extraction (no forms)
├── 3. Mastra Multi-Agent Engine: Personal helper per friend | Admin Agent arbiter | Automatic safety checks
├── 4. Real Map Engine: Leaflet + OpenStreetMap | Fast search | Real pinned spots only (stops if 0 places)
├── 5. Dual Modes: Plan Mode (align dates & schedule) | Trip Mode (next stop countdown & 1-hour rain rescue)
├── 6. Fair Money Split: Auto-logged spending | Who-owes-who cards | Custom splits (exemptions for non-drinkers)
├── 7. Dual-Channel AI: Private helper | Group chat concierge @PlanB | Mobile-first 6-tab PWA
└── 8. Cloudflare Edge Stack: Cloudflare Pages & Workers | Cloudflare D1 SQL | Flexible AI (Workers AI OR Gemini)
```

---

## 2. Ideation & Process

### 2.1 Ideas We Considered

| Idea | What We Decided | Why We Chose or Dropped It |
| :--- | :--- | :--- |
| **A. Plan B Multi-Agent App (Our Choice)** | **Kept (Our Product)** | Replaces 5 messy apps with one clean mobile space. Gives every friend their own AI advocate, while an Admin AI caps the budget to protect the lowest spender and fixes delays in 1 hour. |
| **B. Cloudflare Pages + Workers + D1 Database** | **Kept (Tech Stack)** | Super fast mobile loading worldwide. Saves everything instantly to Cloudflare D1 with zero waiting and zero server setup. |
| **C. Mastra Multi-Agent Framework** | **Kept (AI Framework)** | Keeps AI agents organized in clean TypeScript. Gives each agent memory, lets them talk to each other, and runs automatic tests to prevent hallucinated places. |
| **D. Photo & Voice Scanner ("No-Form" AI)** | **Kept (Easy Input)** | Lets friends drop screenshots from Xiaohongshu, Instagram, or Google Maps, or send quick voice notes. Pulls places, dates, and costs without filling forms. |
| **E. OpenStreetMap + Photon Search** | **Kept (Mapping)** | Fast search-as-you-type autocomplete and tap-to-pin. Completely free with no credit card, no expensive Google Maps fees, and no rate limits. |
| **F. In-Room Chat with @PlanB Concierge** | **Kept (Team Chat)** | Keeps trip planning right inside the chat. Friends can tag `@PlanB` to get instant answers or turn 100+ messy messages into a 1-tap agreed schedule. |
| **G. All-in-One Booking App with Live Flight/Hotel APIs** | **Dropped** | Flight and hotel booking APIs are slow, expensive, and constantly break. Friends book their flights separately anyway; fake booking buttons only make demos fragile. |
| **H. One Big Generic Chatbot Prompt** | **Dropped** | Open-ended chatbots make up closed restaurants and fake prices. A single prompt cannot protect private budgets or keep secret surprise plans hidden. |

---

### 2.1.1 How Our Architecture Evolved

Our system went through three versions based on testing and mentor advice:

```mermaid
flowchart LR
  V1["<b>V1: Booking Super-App</b><br/>❌ Fragile flight/hotel APIs<br/>❌ Checkout errors & rate limits<br/>❌ Zero group consensus"] --> V2["<b>V2: Single-Bot Monolith</b><br/>⚠️ Vercel + Supabase stack<br/>⚠️ Boring manual forms to fill<br/>⚠️ One generic prompt for 4 people<br/>⚠️ No safety test harness"]

  V2 --> V3["<b>V3 FINAL: Cloudflare Multi-Agent Workspace</b><br/>✅ Full Cloudflare stack (Workers + D1 SQL)<br/>✅ Mastra multi-agent framework with auto-tests<br/>✅ Drop photos & voice notes (zero forms)<br/>✅ Personal agent per friend + Admin Arbiter<br/>✅ Flexible AI: Workers AI OR Gemini"]
```

| Feature | V1 (Booking Super-App) | V2 (Single-Bot Setup) | **V3 FINAL (Plan B Multi-Agent)** |
| :--- | :--- | :--- | :--- |
| **Architecture** | Big app calling live booking APIs | One serverless route on Vercel | **Mastra Multi-Agent on Cloudflare** |
| **Hosting** | Traditional server container | Vercel serverless functions | **Cloudflare Pages + Workers (Super fast edge)** |
| **Database** | Heavy PostgreSQL container | Supabase cloud database | **Cloudflare D1 (Fast, lightweight edge SQL)** |
| **How You Input Info**| Scraped booking sites | Boring 4-step manual web forms | **Drop screenshots & voice notes (No forms!)** |
| **AI Helpers** | None (Static booking UI) | One generic prompt for all 4 people | **Your own Personal Agent + Admin Agent for the group** |
| **Group Decisions** | None (Only solo checkout) | Simple lowest-number math | **Admin Agent balances dates, budgets, and rest time** |
| **Safety Testing** | None | Checked prompts manually | **Automated tests to make sure AI follows rules** |
| **AI Engine** | None | Single API call to Gemini | **Selectable: Cloudflare Workers AI OR Google Gemini** |
| **Fixing Delays** | Start flight search all over | Bot rewrites the entire 3-day trip | **1-Hour Quick Fix (only patches the ruined hour)** |

---

### 2.2 System & User Flow Diagrams

All diagrams below show how real friend groups use Plan B, from the first chat to on-the-road travel.

#### 2.2.0 Multi-Agent System Lifecycle: What Each AI Agent Does
**How It Works:** How the multi-agent system cleans up messy inputs, balances friend preferences, caps budgets, schedules real map spots, and rescues ruined hours in real time.

```mermaid
flowchart TD
  subgraph INTAKE["1. Easy Intake (No Boring Forms)"]
    U1["Chat with Personal Agent<br/>(Speak your budget, dates & wishes)"]
    U2["Drop Photos & Voice Notes<br/>(Xiaohongshu / IG / Maps / Audio)"]
    ExtractAgent["📸 AI Scanner (Extractor Agent)<br/>• Pulls places, dates & budget from text/photos<br/>• Auto-creates profile tags & place cards<br/>(Zero form-filling required)"]
    U1 --> ExtractAgent
    U2 --> ExtractAgent
  end

  subgraph PERSONAL_AGENTS["2. Personal AI Agents (Your Advocates)"]
    Agent1["🤖 Personal Agent 1<br/>(Trip Host · Normal Pace · Higher Budget)"]
    Agent2["🤖 Personal Agent 2<br/>(Food Allergies & Halal/Veg Profile)"]
    Agent3["🤖 Personal Agent 3<br/>(Late Sleeper · Easy Pacing)"]
    Agent4["🤖 Personal Agent 4<br/>(🔒 Secret Surprise Wishlist · Lowest Budget)"]
    ExtractAgent --> Agent1
    ExtractAgent --> Agent2
    ExtractAgent --> Agent3
    ExtractAgent --> Agent4
  end

  subgraph ARBITRATION["3. Admin Agent Group Room"]
    AdminAgent["👑 Admin Agent (The Group Arbiter)<br/>• Reviews what all 4 friends want<br/>• Finds common free dates for everyone<br/>• Sets group budget to lowest friend's cap (min-cap)<br/>• Keeps secret surprise stops private"]
    Agent1 <--> AdminAgent
    Agent2 <--> AdminAgent
    Agent3 <--> AdminAgent
    Agent4 <--> AdminAgent
  end

  subgraph ORCHESTRATOR["4. Schedule Planning & Real Map Check"]
    Coord["🎯 Route Planner (Coordinator Agent)<br/>• Checks pins on OpenStreetMap<br/>• Stops if 0 places added (Zero Hallucinations)<br/>• Builds the daily schedule using Admin rules"]
    AdminAgent --> Coord
    D1[(Cloudflare D1 SQL Database)]
    Coord <--> D1
  end

  subgraph EXECUTION["5. Live Trip Mode & 1-Hour Rain Fix"]
    Live["⏱️ Live Next Stop & Time Tracker"]
    Rescue["🚨 1-Hour Plan B Rescue<br/>• Fixes ONLY the delayed 1 hour<br/>• Suggests 3 dry indoor backup spots<br/>• 🔒 Booked flights & hotels stay locked!"]
    Coord --> Live
    Live -->|"Rain / Delay Tapped"| Rescue
    Rescue -->|"One-Tap Fix"| D1
  end

  subgraph CONCIERGE["6. Group Chat & Easy Bill Split"]
    Concierge["💬 Group Chat Concierge (@PlanB)<br/>• Answers trip questions for the group<br/>• Turns 100+ chat messages into a 1-tap agreement"]
    Settle["💰 Smart Bill Splitter<br/>• Logs spending from receipts<br/>• Fair splits (e.g. non-drinkers pay $0)<br/>• Shows who owes who with 1-tap Pay"]
    D1 --> Concierge
    D1 --> Settle
  end
```

---

#### 2.2.1 Why Group Trips Break Down (The Real Story)
**The Problem:** Three common frustrations cause friends to fight and ruin vacations:

```mermaid
flowchart TB
  subgraph CAUSES["Where Group Travel Goes Wrong"]
    C1["Scattered Across 5 Apps<br/>Tickets in email, dates in WhatsApp,<br/>places in Notes, bills in Splitwise"]
    C2["Unspoken Money & Sleep Clashes<br/>Friends argue about budgets and 6 AM alarms<br/>in messy group chats"]
    C3["Fake & Unreliable Travel Bots<br/>Generic chatbots invent closed shops,<br/>wrong prices, and impossible travel times"]
  end

  P["THE CORE BREAKDOWN<br/>No single shared truth for people, dates, budget, and daily plans"]

  subgraph EFFECTS["What Happens on the Trip"]
    E1["Arguments & Overspending<br/>Group compromises up, friends with less money feel stressed"]
    E2["Awkward Money Chasing<br/>Doing complicated math days after getting home"]
    E3["Fake & Closed Places<br/>Itinerary looks nice on paper, but shops are closed in real life"]
    E4["One Rainstorm Destroys the Day<br/>A single delay ruins the whole weekend because there is no backup"]
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
**The Step-by-Step Flow:** How a group of friends plans, travels, and splits costs together smoothly.

```mermaid
flowchart LR
  Step1["1. Quick Invite<br/>Host shows dynamic<br/>QR code or link"] --> Step2["2. Easy Intake<br/>Chat with your AI helper (no forms),<br/>picks dates, budget & pace"]

  Step2 --> Step3["3. Admin Ruling<br/>Finds common dates & caps<br/>budget to lowest friend's limit"]

  Step3 --> Step4["4. Pin Real Places<br/>Friends search & tap favorite spots<br/>on OpenStreetMap canvas"]

  Step4 --> Step5["5. AI Drafts Schedule<br/>Arranges real pinned spots<br/>by travel pace under budget cap"]

  Step5 --> Step6["6. Chat & Concierge<br/>Talk in room, tag @PlanB<br/>to turn debates into 1-tap updates"]

  Step6 --> Step7["7. Live Trip Mode<br/>On the road: Next stop countdown,<br/>1-tap 'Done' or 'Delay'"]

  Step7 --> Step8["8. 1-Hour Rain Rescue<br/>Rain hits? AI patches that 1 hour,<br/>hotels & flights stay 100% locked"]

  Step8 --> Step9["9. Easy Bill Split<br/>Log bills anytime, see who<br/>owes whom in fewest payments"]
```

---

#### 2.2.3 End-to-End Activity Flow
**How It Works Behind the Scenes:** How friends, personal AI helpers, the Admin Agent, and live trip mode work together smoothly.

```mermaid
flowchart TD
  subgraph THE_GANG["👥 Travel Group Friends"]
    Start(["Start Planning"]) --> Join["Join via QR Code Scan or Link"]
    Join --> ChatIntake["Chat with Personal AI Helper<br/>(Voice, text, or drop screenshots, zero forms)"]
    SecretChoice{"Planning a surprise?"}
    ChatIntake --> SecretChoice
    SecretChoice -->|"Yes: Birthday / Proposal"| HideSpot["Turn on 'Hide from group'<br/>(Kept hidden on friends' screens)"]
    SecretChoice -->|"No: Normal Stop"| PublicSpot["Save as Public Stop"]
    HideSpot --> PinSpots["Search & Pin Real Places on Map<br/>(Verified attractions, cafes & sights)"]
    PublicSpot --> PinSpots
    PinSpots --> GroupChat["Talk in Group Chat<br/>(Share pins & discuss timing)"]
    GroupChat --> AskSummary["Tag '@PlanB summarize opinions'<br/>(Admin Agent finds the agreement)"]
  end

  subgraph ADMIN_AGENT_ENGINE["👑 Admin Agent & Mastra Engine"]
    ChatIntake --> SubmitProps["Personal Agents Submit Each Friend's Wishes"]
    SubmitProps --> CalcOverlap["Finds Dates When Everyone Is Free"]
    CalcOverlap --> LockBudget["Caps Group Budget to Lowest Spender:<br/>min(all member budgets) — protects friend with least money"]
    LockBudget --> ReadyDashboard["Shows Group Alignment Dashboard"]
    ReadyDashboard --> ConfirmCity{"Destination Confirmed<br/>by Group?"}
    ConfirmCity -->|"Yes: Confirmed"| CheckPins{"Any real places pinned?"}

    AskSummary --> SummarizeDebate["Admin Agent Summarizes Chat:<br/>1. Agreed food & activity spots<br/>2. Balanced rest times & sleep schedules<br/>3. Action: 1-tap update to schedule"]
    SummarizeDebate --> PostDiff["Post Agreement Card into Chat"]
    PostDiff -.-> ConfirmCity

    CheckPins -->|"No: 0 places"| StopAlert["🛑 Safety Guardrail Stops AI:<br/>'No places pinned yet! Please add spots first.'<br/>(Refuses to make up fake places)"]
    StopAlert -.-> PinSpots

    CheckPins -->|"Yes: Real pins exist"| BuildDays["Builds Daily Schedule:<br/>- Arranges real places by travel pace<br/>- Secretly routes surprise spot without leaking name<br/>- Keeps total estimated spend under lowest budget cap"]
  end

  subgraph ON_THE_ROAD["🚗 Live Trip & Fair Bill Splitting"]
    BuildDays --> GoTrip["Turn on Live Trip Mode on Travel Day"]
    GoTrip --> ActiveCard["NEXT STOP Card & Time Countdown<br/>(Where to go next, arrival time & ETA)"]

    ActiveCard --> RoadStatus{"What happens on the road?"}
    RoadStatus -->|All Good| HitDone["Tap 'Done' (Arrived)"]
    RoadStatus -->|Rain or Delay| HitDelay["Tap 'Delay' (+30m)<br/>Reason: Sudden rain or closed attraction"]

    HitDelay --> SaveStays["🔒 Protect Key Anchors:<br/>Booked hotels & flights stay 100% locked!"]
    SaveStays --> QuickFix["AI: Suggests 3 dry indoor backup spots<br/>from your saved place pool"]
    QuickFix --> ActiveCard

    HitDone --> LogBill["Log Expense via Voice, Text, or Receipt<br/>(Payer, amount & who owes is auto-calculated)"]
    LogBill --> SettleDebt["Smart Bill Splitter:<br/>Directly shows who owes who with 1-tap Mark Paid"]
    SettleDebt --> FinishTrip(["Trip Finished & Fairly Paid Up!"])
  end
```

---

#### 2.2.4 Core Feature Flows (How Each Part Works)

##### 1. Finding Common Ground (Shared Dates & Lowest Budget Cap)
**The Problem Solved:** Friends spend days debating dates and budgets without deciding. Plan B automatically finds overlapping free days and caps the budget to the lowest friend's limit so nobody overspends.

```mermaid
flowchart TD
  F1["Member 1 (Host)<br/>Dates: Window A · Higher Budget · Standard Pace"] --> MatchEngine["Plan B Matching Engine"]
  F2["Member 2<br/>Dates: Window B · Medium Budget · Balanced Pace"] --> MatchEngine
  F3["Member 3<br/>Dates: Window C · Lowest Budget Cap · Easy Pace"] --> MatchEngine

  MatchEngine --> Win["Finds Common Free Days:<br/>Dates that work for all friends"]
  MatchEngine --> Cap["Caps Budget to Lowest Spender:<br/>min(all member budgets) — protects friend with least money"]
  MatchEngine --> Rules["Checks Everyone's Needs:<br/>Balanced wake-up hours · Halal / veg dietary checks"]

  Win --> SummaryCard["Shows Group Alignment Card<br/>(Ready status · Common dates · Group budget cap)"]
  Cap --> SummaryCard
  Rules --> SummaryCard

  SummaryCard --> ConfirmDest{"Destination Confirmed?"}
  ConfirmDest -->|Yes: Destination Locked| UnlockPlan["Unlock Map & Schedule Planning ✓"]
```

---

##### 2. Keeping a Secret Surprise (Hidden Wishlists)
**The Problem Solved:** Coordinating a surprise celebration or hidden wishlist stop usually leaks in group chats. Plan B lets you add private spots that friends can't see, while the AI quietly plans them into the day's route.

```mermaid
flowchart TD
  Traveler["Proposing Friend"] --> Propose["Adds Secret Spot (e.g. Birthday dinner / Proposal)"]
  Propose --> Switch["Turn on: 'Hide from group' (Surprise)"]

  Switch --> SharedView["Friends' Screen:<br/>Map shows generic '🔒 Surprise Activity'<br/>Real name and coordinates stay hidden!"]
  Switch --> PrivateView["Your Private Screen:<br/>Shows full place details & private badge"]

  Switch --> ServerAI["Mastra AI Engine"]
  ServerAI --> SafeInstruction["System Rule:<br/>'Route this stop into the day, but NEVER reveal its name to the group.'"]
  SafeInstruction --> DraftRoute["Day Route Created:<br/>Seamless day plan, surprise stays 100% secret!"]
```

---

##### 3. Pinning Real Places on OpenStreetMap
**The Problem Solved:** Eliminates reliance on commercial map API keys or brittle booking engines. Search real venues via fast Photon autocomplete or tap anywhere on the interactive map canvas to drop custom pins.

```mermaid
flowchart TD
  HowToAdd{"How Friend Adds a Stop"}

  HowToAdd -->|Search Bar| TypeSearch["Type place name in search bar"]
  TypeSearch --> AutoList["Instant Photon Suggestions<br/>(Real street addresses via OpenStreetMap)"]
  AutoList --> PickOne["Select verified venue"]

  HowToAdd -->|Tap Map Canvas| TapMap["Tap anywhere on Leaflet map canvas"]
  TapMap --> CustomPin["Drops custom pin (e.g. Scenic Viewpoint)"]

  PickOne --> SavePool["Save to Trip Place Pool<br/>(Saved pins become approved spots for the trip)"]
  CustomPin --> SavePool

  SavePool --> RenderPin["Shows Numbered Pin on Map<br/>(Pin 1, Pin 2, Pin 3...)"]
  RenderPin --> AddDrawer["Bottom Drawer: '+ Add to Itinerary Pool'"]
```

---

##### 4. AI Drafting the Schedule (Real Places Only, No Fake Shops)
**The Problem Solved:** Generic travel bots make up closed shops or fake prices. Plan B's engine is strictly constrained: it only sequences venues that friends actually pinned, and halts execution immediately if zero places exist.

```mermaid
flowchart TD
  ClickGen["Traveler taps: 'Generate Itinerary'"] --> CheckCount{"How many places in trip pool?"}

  CheckCount -->|0 Places Added| HaltEngine["🛑 Safety Guardrail Stops AI:<br/>'No places pinned yet! Please add spots first.'<br/>(Refuses to make up fake places)"]

  CheckCount -->|"Places Exist in Pool"| PrepareData["Feed Trip Parameters to Mastra Engine:<br/>- Pinned Places: Real verified candidate pins<br/>- Lowest Budget Cap: min(all member caps)<br/>- Group Pace: Harmonized travel pace"]

  PrepareData --> MastraRun["Mastra Coordinator Engine:<br/>Calculates logical route via active provider<br/>(Workers AI OR Gemini)"]
  MastraRun --> VerifyOutput["Verify All Venue IDs Match Real Pinned Spots"]
  VerifyOutput --> RenderSchedule["Render Day-by-Day Itinerary Cards<br/>(Chronological stops, locked stays, transit estimates)"]
```

---

##### 5. The Plan B Rescue (Sudden Rain or Delay in Trip Mode)
**The Problem Solved (Our Core Innovation):** When sudden disruptions occur (heavy rain, transit delays, or temporary venue closures), traditional apps force rewriting the entire multi-day schedule. Plan B patches **only that specific 1-hour slot**, keeping booked hotel check-ins and return flights 100% locked!

```mermaid
flowchart TD
  LiveTrip["Live Trip Mode<br/>Active Stop: Outdoor Activity (09:00 AM)"] --> Storm{"Disruption Happens!<br/>Sudden rainstorm or venue closure"}

  Storm --> DelayTap["Member taps: 'Delay (+30m)' or 'Skip'"]
  DelayTap --> LockRule["🔒 Protect Key Anchors:<br/>- Booked Hotel Check-in: LOCKED<br/>- Return Flight / Transit: LOCKED<br/>(Never moved or rescheduled!)"]

  LockRule --> SearchIndoor["AI checks unused saved pins for nearby indoor spots"]
  SearchIndoor --> GiveOptions["Present 3 Dry Indoor Plan B Backups:<br/>1. Nearby Indoor Cultural / Heritage Venue<br/>2. Nearby Indoor Cafe / Gallery<br/>3. Nearby Covered Sight"]

  GiveOptions --> PickFix["Group selects preferred indoor backup"]
  PickFix --> PatchSlot["Patches ONLY the disrupted time slot in database<br/>Rest of the trip stays perfectly intact!"]
```

---

##### 6. Fair & Painless Bill Splitting (Zero Awkward Money Talks)
**The Problem Solved:** Traditional split apps require tedious post-trip manual receipt entries. In Plan B, travelers never have to fill out accounting forms—simply tell the AI Agent via voice note, chat text, or receipt photo with any custom split exemptions. The Money dashboard directly displays the exact settlement transfers required!

```mermaid
flowchart TD
  Receipt1["Member A tells AI Agent: Shared dining expense<br/>(Custom split: Member D zero-share exemption)<br/>(Voice memo, chat text, or receipt photo)"] --> AIParse["AI Extractor & Ledger Agent:<br/>Auto-parses payer, amount & custom exemptions"]
  Receipt2["Member B tells AI: Transit expense (Even 4-way split)"] --> AIParse
  AIParse --> MoneyLedger["Trip Ledger: Total Group Spend"]

  MoneyLedger --> BalanceMath["Calculate Fair Share for Each Person"]
  BalanceMath --> S1["Member A: Creditor (+Balance)"]
  BalanceMath --> S2["Member B: Creditor (+Balance)"]
  BalanceMath --> S3["Member C: Debtor (-Balance)"]
  BalanceMath --> S4["Member D: Debtor (-Balance)"]

  S1 --> SmartSettlement["Smart Debt Simplifier<br/>(Calculates the fewest direct payments)"]
  S2 --> SmartSettlement
  S3 --> SmartSettlement
  S4 --> SmartSettlement

  SmartSettlement --> Pay1["1. Member C transfers to Member A"]
  Pay1 --> CardUI["Direct Display: Exact Amounts to Pay<br/>Tap Mark Paid button to settle in 1 second!"]
  SmartSettlement --> Pay2["2. Member D transfers to Member A"]
  Pay2 --> CardUI
  SmartSettlement --> Pay3["3. Member D transfers to Member B"]
  Pay3 --> CardUI
```

---

##### 7. In-Room Group Chat with Admin Agent @PlanB Concierge & Consensus Summarizer
**The Problem Solved:** Endless unstructured chat threads leave trip decisions buried and forgotten. Inside Plan B's chat room, friends can mention `@PlanB` to get instant answers or summarize debates into actionable, one-tap schedule updates.

```mermaid
flowchart TD
  subgraph THE_CHAT["💬 In-Room Group Chat"]
    M1["Member A: Shares a pinned dining venue"]
    M2["Member B: Requests relaxed afternoon pacing"]
    M3["Member C: Confirms spend must stay under lowest budget cap"]
    AskAI["Host: '@PlanB summarize what we agreed on'"]
  end

  subgraph THE_CONCIERGE["👑 Admin Agent Concierge (@PlanB)"]
    DetectQuery{"Did someone mention @PlanB?"}
    ParseChat["Admin Agent Reads Chat History & Member Constraints:<br/>- Agreed stop: Candidate venue from place pool<br/>- Pacing consensus: Relaxed afternoon buffer<br/>- Budget check: Confirmed within lowest member budget ceiling"]
    CreateDiff["Generate Consensus Summary Card with<br/>Apply to Itinerary action button"]
  end

  M1 --> DetectQuery
  M2 --> DetectQuery
  M3 --> DetectQuery
  AskAI --> DetectQuery

  DetectQuery -->|Yes: @PlanB| ParseChat
  ParseChat --> CreateDiff
  CreateDiff --> PostFeed["Broadcast Summary Card directly into Chat Feed!"]
  PostFeed --> AllSee["All 4 friends see the exact same agreement,<br/>1 tap updates the schedule for everyone!"]
```

---

#### 2.2.5 Complete End-to-End User Flow
**The Complete App Flow:** Seamless progression from room invite to live travel and bill settlement.

```mermaid
flowchart TD
  Login["Welcome & Sign In<br/>Google, Apple or Magic Link"] --> TripsHub["Trips Hub<br/>Create Trip or Scan QR Code"]
  TripsHub --> Invite["Instant Invite<br/>Dynamic QR Code & Share Link"]
  TripsHub --> Roster["Trip Room: Member Roster<br/>Host & Member Readiness Status"]
  Roster --> EasyIntake["Easy Chat Intake<br/>Chat with Personal AI (no forms),<br/>Extracts dates, budget, pace & secrets"]
  EasyIntake --> AdminRuling["Admin AI Decision<br/>Finds common free dates &<br/>caps budget to lowest friend (min-cap)"]
  AdminRuling --> DestCheck{"Admin AI Confirms Destination?"}
  DestCheck -->|No| Roster
  DestCheck -->|Yes| PlanHub["Trip Plan Hub<br/>Schedule, Map, AI Chat & Money"]
  PlanHub --> Map["Map Canvas<br/>Search & Pin Real Places"]
  PlanHub --> PersonalAI["Personal AI Console<br/>1-on-1 Chat & Budget Check"]
  PlanHub --> GroupChat["Group Chat & @PlanB<br/>Team Chat, Agent Deliberation & Rulings"]
  Map --> PlaceCheck{"Places Pinned > 0?"}
  PlaceCheck -->|No: Halt| Map
  PlaceCheck -->|Yes| Itinerary["Daily Itinerary<br/>Admin-Sequenced Real Stops"]
  GroupChat -.->|"Apply Consensus Ruling"| Itinerary
  PersonalAI -.->|"Check & Simulate"| Itinerary
  Itinerary --> LiveTrip["Live Trip Mode<br/>NEXT STOP: Active Countdown"]
  LiveTrip --> DisCheck{"Rain or Delay?"}
  DisCheck -->|Yes: Delay| Replan["1-Hour Rain Rescue<br/>Indoor backup, stays locked"]
  Replan --> LiveTrip
  DisCheck -->|No: Settle up| Money["Money & Bills<br/>Zero-Form Ledger & Who-Owes-Who"]
  TripsHub --> Profile["Profile: You<br/>Travel Pace & Dietary Defaults"]
```

> **Persistent Bottom Navigation Tabs (Always Accessible):**
> `Trips · Map · Plan · Money · Chat · You`

---

#### 2.2.6 How Our Architecture Evolved
**Why We Built It This Way:** We deliberately tossed out fragile parts (like commercial booking checkouts and strict geocoders) to keep the app 100% reliable, blazing fast at the edge, and powered by teamwork between AI agents.

```mermaid
flowchart LR
  V1["V1: Booking Super-App<br/>(Live flight/hotel checkout APIs)<br/>❌ Failed: Broken checkouts & rate limits"] --> V2["V2: Single-Agent Monolith<br/>(Vercel + Supabase + Manual Forms)<br/>⚠️ Limited: High friction & single prompt"]
  V2 --> V3["V3 FINAL: Cloudflare Multi-Agent Workspace<br/>Cloudflare (Workers + D1) + Mastra + Selectable AI<br/>✅ Success: Zero forms, personal agents & edge speed!"]
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

| Date | Mentor | Feedback Received | What Was Changed |
| :--- | :--- | :--- | :--- |
| `2026-09-06` | **Jarod Tan** | • **AI Agent Harness:** Use a dedicated AI agent harness for evaluation, structured testing, and reliable execution.<br/>• **Cloudflare Sandbox Hosting:** Host backend AI services and edge agents on Cloudflare Workers for isolated, high-speed execution.<br/>• **Simpler Documentation with More Diagrams:** Keep documentation straightforward and easy to read, with clear visual diagrams instead of heavy technical buzzwords.<br/>• **Distinctive Feature Focus:** Highlight killer features that make Plan B special (like the 1-hour rain rescue, secret wishlist shield, and lowest-spender budget cap). | 1. **Adopted Mastra (`@mastra/core`) Agent Harness:** Built structured multi-agent workflows, tool calling, and automated test suites to verify safety guardrails (like stopping if no places are pinned, and protecting booked hotels).<br/>2. **Migrated to Cloudflare & Cloudflare D1:** Deployed the frontend and edge agents on Cloudflare Pages/Workers, using Cloudflare D1 for instant serverless database storage, with selectable AI (Workers AI or Gemini).<br/>3. **Added Zero-Form Intake:** Enabled travelers to drop social media screenshots, voice memos, and casual chat messages so nobody ever has to fill out annoying forms.<br/>4. **Upgraded to Multi-Agent System:** Gave each friend their own autonomous Personal AI to protect their budget and free dates during group planning. |

---

## 3. Design & Prototype

### Mobile Interface & Navigation (Phone-First PWA)

Plan B is built as a phone-friendly web app designed for effortless one-handed use on mobile devices (390px viewport). It features **6 persistent bottom navigation tabs** that stay easily accessible at all times:

```mermaid
flowchart TD
  subgraph TABS["📱 6 Persistent Bottom Navigation Tabs"]
    T1["<b>1. Trips</b><br/>My Trips, Create & QR Invites"]
    T2["<b>2. Map</b><br/>OpenStreetMap & Real Pins"]
    T3["<b>3. Plan</b><br/>Intake, Alignment & Daily Schedule"]
    T4["<b>4. Money</b><br/>Zero-Form Bills & Settlement"]
    T5["<b>5. Chat</b><br/>Team Chat & @PlanB Concierge"]
    T6["<b>6. You</b><br/>Profile, Pace & Dietary Rules"]
  end

  subgraph STAGES["🗺️ Core Experience Modules"]
    S1["<b>Room Entry & Invites:</b> Instant QR scan or link join, zero app store downloads"]
    S2["<b>Conversational Intake:</b> Voice memos, chats & screenshots, zero manual forms"]
    S3["<b>Grounded Route Planning:</b> OpenStreetMap pins, strictly verified stops"]
    S4["<b>Live Trip & 1-Hour Rescue:</b> Next stop countdown & 1-hour indoor rain patch"]
    S5["<b>Effortless Bill Splitting:</b> Voice/photo logging & direct who-owes-who cards"]
    S6["<b>Team Collaboration:</b> Real-time in-room chat with @PlanB consensus cards"]
  end

  T1 --> S1
  T3 --> S2
  T2 --> S3
  T3 --> S4
  T4 --> S5
  T5 --> S6
  T6 --> S6
```

### Core Interface Modules & User Experience

#### 1. Trips Hub & Instant Invites (`Trips` Tab)
- **Active Trips Overview:** View ongoing and upcoming getaways with clear `Planning` or `Live` status tags, or tap `+ New Trip` to start a new adventure.
- **Dynamic QR Code Invites:** The host displays a large, clear on-screen QR code or shares an instant link. Friends simply point their phone camera at the screen to join the trip room in 2 seconds—no app store downloads needed.

#### 2. Planning & Conversational Intake (`Plan` Tab)
- **Member Roster & Readiness:** Displays all friends in the room and tracks who has finished sharing their preferences with their Personal AI.
- **No-Form Intake (Voice, Text & Screenshots):** Tell your Personal AI what you want, send voice memos, or drop screenshots from Xiaohongshu, Instagram, or Google Maps. The AI automatically captures dates, spending limits, travel pace, food restrictions, and secret wishlists.
- **Admin AI Arbitration:** The central Admin AI resolves date conflicts, harmonizes travel pace, and strictly caps the group budget to the friend with the least money (`min-cap`).
- **Grounded Daily Itinerary:** Step-by-step chronological timeline built strictly from real member-added pins, with green `🔒 Locked` badges on booked accommodations and return flights.
- **Secret Wishlist Shield:** Special surprise stops (like birthday dinners or proposal spots) are scheduled seamlessly into the route while hiding the venue name from friends' views until arrival.

#### 3. Real Map Canvas (`Map` Tab)
- **OpenStreetMap & Leaflet Canvas:** Fast, interactive map with search-as-you-type fuzzy autocomplete powered by Photon.
- **Tap-to-Pin Anywhere:** Search real street addresses or tap directly on the map to drop custom pins. Only real places you actually pinned enter the itinerary pool, eliminating hallucinated or closed shops.

#### 4. Shared Expenses & Settlement (`Money` Tab)
- **Zero-Form Shared Ledger:** Auto-logs group spending from voice memos, chat notes, or receipt photos sent to the AI, supporting custom split rules (e.g. non-drinkers pay $0).
- **Direct Who-Owes-Who Settlement:** Clean cards showing the exact calculated payments between debtors and creditors, worked out so friends make the fewest transfers possible, with a 1-tap `[Mark Paid]` button.

#### 5. Group Chat & AI Concierge (`Chat` Tab)
- **Real-Time Group Chat:** Live in-room messaging powered by WebSockets, with interactive venue cards, friend avatars, and automatic trip event notifications.
- **In-Room Concierge (`@PlanB`):** Tag `@PlanB` in the chat to synthesize long debates into a tidy consensus card that updates the shared itinerary with a single tap.

#### 6. Personal AI Console & Profile (`You` Tab)
- **Private 1-on-1 AI Helper:** Private chat with your dedicated Personal AI. Ask sensitive questions, check food allergies, or log expenses by voice or photo without cluttering the group chat.
- **Traveler Profile & Preferences:** Stores your personal bio, travel stats, and global defaults (travel pace and dietary restrictions) that automatically pre-populate whenever you join a new trip room.

#### 7. Live Trip Mode & 1-Hour Plan B Rescue
- **On-the-Road Dashboard:** Automatically activates during travel days with a pulsing `NEXT STOP` banner, live countdown timer, and quick action controls (`Done`, `Delay`, `Skip`).
- **1-Hour Rain Rescue (The Plan B Twist):** When sudden rain or delays hit, tapping `Delay` opens the rescue modal. It keeps all booked hotel check-ins and flights 100% locked, while replacing only that disrupted 1-hour slot with 3 dry indoor backup spots from your saved pool.

---

## 4. What Makes It Different

```mermaid
flowchart LR
  subgraph TRAD["❌ Traditional Ways (Wanderlog, Splitwise, Chatbots)"]
    direction TB
    T1["<b>Data Intake:</b> Painful 4-step forms & manual typing"]
    T2["<b>Budget:</b> High spenders drag everyone into overspending"]
    T3["<b>Places:</b> Generic travel bots invent fake/closed shops"]
    T4["<b>Delays:</b> 1 rainstorm forces redoing the entire 3 days"]
    T5["<b>Bills:</b> Complex post-trip Splitwise math days later"]
    T6["<b>Chat:</b> 100+ unread WhatsApp messages & lost plans"]
  end

  subgraph PB["✅ The Plan B Advantage (Phone-First PWA)"]
    direction TB
    P1["<b>Data Intake:</b> Drop screenshots & voice notes, zero forms"]
    P2["<b>Budget:</b> Auto-locks ceiling to lowest spender (min-cap)"]
    P3["<b>Places:</b> OpenStreetMap, strictly pinned real places"]
    P4["<b>Delays:</b> 1-hour surgical patch, hotels/flights stay locked"]
    P5["<b>Bills:</b> Tell AI & directly see who owes who how much"]
    P6["<b>Chat:</b> @PlanB summarizes team consensus in 1 tap"]
  end

  TRAD -.->|"Upgraded by Plan B"| PB
```

| Feature | Traditional Travel Apps (Wanderlog, TripIt) | Group Chat + Splitwise | Generic Travel AI Bots | **Plan B (Our Solution)** |
| :--- | :--- | :--- | :--- | :--- |
| **Sharing Your Preferences** | Manual forms & email forward | Messy texts lost in chat | Needs long manual prompt typing | **Zero Forms: Drop screenshots, voice notes, or chat naturally** |
| **AI System Design** | None (Static UI) | None | Single generic chatbot | **Multi-Agent: Dedicated AI for each friend + Admin AI Arbiter** |
| **Building the Itinerary** | Drag-and-drop on desktop | None; text in chat notes | Invents fake shops & wrong prices | **Real Places Only: Built strictly from pins you added** |
| **Budget Protection** | Passive cost display; ignores caps | Retroactive math after overspending | Ignores budgets or makes up prices | **Fair Budget Cap: Group ceiling locked to lowest member cap** |
| **Secret & Surprise Stops** | Not supported; everything is public | Leaked immediately in group chat | N/A | **Secret Wishlist Shield: AI schedules it without showing friends the venue** |
| **Rain & Delay Fix** | Manually reschedule multiple days | Panic in chat; manual scrambling | Re-writes the whole trip from scratch | **1-Hour Rain Fix: Swaps only the ruined hour; stays & flights stay locked** |
| **Splitting Bills** | Paywalled or requires another app | Messy math days after the trip | None | **Zero-Form Bill Split: Log by voice or receipt; screen shows who owes who** |
| **Group Communication** | External (WhatsApp / Telegram) | Messy, noisy, easy to miss things | Solo 1-player chat | **In-App Group Chat: @PlanB reads chat and summarizes consensus in 1 tap** |

### Key Product Highlights:
1. **Zero-Form Experience (Chat, Voice & Photo Drop):** Nobody wants to fill out forms on vacation. Just talk to your Personal AI, send a quick voice memo, or drop a screenshot or receipt photo. The AI automatically grabs the dates, places, and bill splits.
2. **Your Own Personal AI Helper:** Every traveler gets a dedicated AI helper in the app that remembers their personal budget, travel pace, food rules, and secret wishlists.
3. **The Admin AI (Fair Group Arbiter):** When friends disagree on dates or times, the Admin AI steps in, resolves the conflict fairly, enforces the budget cap, and keeps everyone aligned.
4. **Lowest-Spender Budget Cap (`min-cap`):** Group trips get awkward when higher spenders pick pricey places. Plan B caps the group budget to match the friend with the least to spend (e.g. RM 450), so everyone travels comfortably without financial stress.
5. **Secret Surprise Shield:** Want to plan a birthday surprise or proposal? Turn on the secret toggle. The AI routes the stop into the day's trip smoothly without revealing the venue name to your friends.
6. **No Fake Places Guardrail:** The AI only organizes places you actually care about—it never hallucinates fake or closed spots. If no places are pinned, the AI stops and reminds you to drop real pins on the map first.
7. **1-Hour Rain & Delay Rescue (The Core Twist):** When sudden rain hits or an attraction is closed, don't throw away your whole 3-day itinerary. Plan B swaps out just that 1 ruined hour with dry indoor backups, keeping your booked hotels and flights untouched.
8. **Private AI + In-Room Group Concierge:** You have a private 1-on-1 chat with your own AI to ask personal questions or log expenses, plus `@PlanB` in the group chat to summarize team decisions.
9. **No-Math Bill Splitting:** Forget confusing spreadsheets. After logging purchases, the app shows clear Who-Owes-Who cards with exact payment amounts and a 1-tap `Mark Paid` button.
10. **Fast Edge Tech & Tested Safety:** Runs at sub-millisecond edge speeds on Cloudflare with automated test suites that guarantee your locked bookings and budget caps never break.

---

## 5. Technical Architecture & Feasibility

### Tech Stack

| Layer | Technology Chosen | Why We Chose It | Constraints & How We Address Them |
| :--- | :--- | :--- | :--- |
| **Frontend Framework** | **Nuxt 3 (Vue 3, TypeScript)** | Fast mobile web app, smooth map rendering, and works right in your phone's browser without downloading an app. | Leaflet maps need the browser window. We wrap map components in Nuxt's `<ClientOnly>` tags so they load seamlessly. |
| **Edge Hosting & Runtime** | **Cloudflare Pages & Workers** | Instant global speed, zero server maintenance, and near-instant load times. | Serverless CPU limits. Solved by streaming AI responses and running background tasks smoothly. |
| **Edge Database** | **Cloudflare D1 (Serverless SQL)** | Fast SQL database running right at the edge with instant response times. | Multiple people writing at the same time. Handled cleanly by organizing data per trip room so friends never lock each other out. |
| **Multi-Agent Framework & Harness** | **Mastra (`@mastra/core`)** | Orchestrates our AI agents with structured workflows, agent memory, and automated test checks. | Multi-agent communication speed. Kept fast using lightweight typed messages and strict guardrails. |
| **AI Inference Engine (Selectable: Choose One)** | **Option A: Cloudflare Workers AI<br/>OR<br/>Option B: Google Gemini 2.5 Flash** | **Pluggable provider options (pick either one):**<br/>• **Option A (Workers AI):** 100% Cloudflare edge native, 10,000 free Neurons/day, zero external API keys.<br/>• **Option B (Gemini 2.5 Flash):** High-capacity free tier (1,500 req/day), powerful vision for social media screenshots & receipts.<br/>*Switch anytime via a single setting: `AI_PROVIDER=workers-ai` or `AI_PROVIDER=gemini`.* | Quota limits or platform lock-in. Solved cleanly by letting you switch between providers with a single setting without touching any code. |
| **Realtime & Room Sync** | **Cloudflare WebSockets / Realtime** | Instant updates for in-room group chat, new map pins, and live delay alerts. | Active connection limits. Handled cleanly by grouping connections per active trip room. |
| **Mapping & Search** | **Leaflet + OpenStreetMap + Photon** | Free, open-source map and search. Smooth map interactions and instant type-ahead search with zero API keys or monthly bills. | Missing obscure spots. Solved by letting you tap directly anywhere on the map canvas to drop custom pins. |

---

### 5.1 AI Provider Options (Mutually Exclusive Architecture)

Plan B does not run multiple AI providers at the same time. Instead, you pick **either Option A or Option B** using a single environment variable (`AI_PROVIDER`):

| Comparison Dimension | Option A: Cloudflare Workers AI (Edge Native) | Option B: Google Gemini 2.5 Flash (API Key) |
| :--- | :--- | :--- |
| **Architectural Role** | **100% Cloudflare Native Stack** | **High-Capacity Multimodal Specialist** |
| **Pricing & Free Tier** | **Free (10,000 Neurons / day)**, no credit card required | **Free (1,500 requests / day)**, no credit card required |
| **Hosting Location** | Runs directly inside Cloudflare Workers edge network | Direct API call to Google AI Studio |
| **Primary Strength** | Blazing-fast edge speed, zero external API keys needed | Superior vision for screenshots (Xiaohongshu/IG) & receipts, 1M context |
| **Ideal Use Case** | Deployments wanting a pure, self-contained Cloudflare stack | High-volume hackathon testing and heavy receipt photo uploads |
| **Environment Switch** | `AI_PROVIDER="workers-ai"` | `AI_PROVIDER="gemini"` |

---

### Data Flow & Execution Pipeline

```mermaid
flowchart LR
  In["<b>1. Traveler Input</b><br/>Screenshots, Voice, Chats"] --> EdgeRoute["<b>2. Cloudflare Edge</b><br/>Workers & WebSockets"]
  EdgeRoute --> AgentEngine["<b>3. Mastra Multi-Agent</b><br/>Personal Agents & Admin Arbiter"]
  AgentEngine --> Guardrails["<b>4. Safety Guardrails</b><br/>Min-Cap Budget & 0-Place Halt"]
  Guardrails --> D1Storage["<b>5. Cloudflare D1 SQL</b><br/>Real-Time State Persistence"]
  D1Storage --> PWAView["<b>6. Phone PWA UI</b><br/>Interactive Maps & Live Cards"]
```

---

### System Architecture Diagram

```mermaid
flowchart TD
  subgraph CLIENT["Client Layer: Phone-First PWA (390px Mobile Viewport)"]
    UI["Nuxt 3 PWA UI<br/>(Vue 3 · Pinia · Tailwind CSS)"]
    MapClient["Leaflet OSM Engine<br/>(<ClientOnly> Interactive Canvas)"]
    RTClient["Realtime WebSocket Client<br/>(Live Chat & Disruption Broadcast)"]
  end

  subgraph EDGE["Cloudflare Edge Services (Pages & Workers)"]
    AuthService["Account & Login Service<br/>Profile & Session Management"]
    IntakeService["Photo & Voice Scanner<br/>Pulls Dates, Budget & Places"]
    AgentService["Agent Room Coordinator<br/>Multi-Agent Negotiation"]
    ReplanService["1-Hour Delay Rescue<br/>Patches Broken Hour with Backups"]
    SearchService["Map Search Service<br/>Instant Place Autocomplete"]
    DebtService["Bill Splitter<br/>Who-Owes-Who Debt Settlement"]
  end

  subgraph MASTRA["Mastra Multi-Agent Layer (@mastra/core)"]
    ExtractAgent["Scanner Agent<br/>(Turns Voice, Chat & Photos into Cards)"]
    PersonalAgents["Personal Agents (x4)<br/>(1 Helper per Friend: Budget & Pace)"]
    AdminAgent["👑 Admin Agent (The Arbiter)<br/>(Balances Group Dates, Budget & Schedule)"]
    CoordAgent["Coordinator Agent<br/>(Safety Stop & Real Places Sequencer)"]
    EvalHarness["Test Harness<br/>(Automated Safety & Accuracy Checks)"]
  end

  subgraph STORAGE["Cloudflare Edge Storage Layer"]
    D1[(Cloudflare D1 SQL Database<br/>Fast Serverless Edge Database)]
  end

  subgraph EXTERNAL["AI & Map Services"]
    LLM["AI Engine<br/>Text, Voice & Image Processing"]
    MapEngine["Map & Location Engine<br/>Place Search & Map Rendering"]
  end

  UI --> AuthService
  UI --> IntakeService
  UI --> AgentService
  UI --> ReplanService
  UI --> DebtService
  UI --> SearchService
  UI --> MapClient
  UI <--> RTClient

  IntakeService --> ExtractAgent
  AgentService --> PersonalAgents
  PersonalAgents <--> AdminAgent
  AdminAgent --> CoordAgent
  ReplanService --> CoordAgent
  CoordAgent <--> EvalHarness

  ExtractAgent --> LLM
  PersonalAgents --> LLM
  AdminAgent --> LLM
  CoordAgent --> LLM
  SearchService --> MapEngine

  CoordAgent -.->|Save Verified Stops| D1
  ReplanService -.->|Keep Stays Locked| D1
  AuthService --> D1
  DebtService --> D1
```

---

### Build Plan & Scope

#### In-Scope (What We Build):
- [x] Clean phone-first login with personal travel preferences (pace, dietary restrictions).
- [x] Trip room lifecycle with instant QR code generation, camera scanning, and direct link invite.
- [x] **Conversational Intake (Zero Forms):** Chat naturally with your Personal AI via voice, text, or drop screenshots; automatically parses dates, budget, pace, and wishlists.
- [x] **Autonomous Personal AI per Member:** Dedicated **Mastra** agent per traveler guarding individual budgets, pace, and secret wishlists.
- [x] **Admin Agent (Master Arbiter & Decider):** Centralized room arbitrator that evaluates personal agent proposals, enforces the lowest-budget cap (`min-cap`), and decides authoritative schedule updates.
- [x] OpenStreetMap with Photon fuzzy autocomplete search and direct tap-to-pin coordinate saving.
- [x] **Mastra Coordinator Agent & Safety Harness:** Strict allow-list sequencing, hard 0-place halt safety stop, and automated test checks.
- [x] **Cloudflare Full-Stack Architecture:** Cloudflare Pages & Workers hosting with **Cloudflare D1** serverless distributed SQL database.
- [x] **Pluggable AI Architecture (Choose One):** Selectable between **Option A: Cloudflare Workers AI** (100% native edge, 10,000 free Neurons/day) OR **Option B: Google Gemini 2.5 Flash** (1,500 free requests/day, deep multimodal vision) via a single environment setting (`AI_PROVIDER`).
- [x] Plan Mode (trip preparation) and Trip Mode (live next-stop countdown) states.
- [x] 1-hour delay rescue modal replacing only the disrupted hour while preserving locked flights and hotel stays.
- [x] Zero-form shared bill logging with direct Who-Owes-Who settlement cards (Balance Equalizer).
- [x] Real-time in-room team group chat with place card sharing, live delay alerts, and Admin Agent (`@PlanB`) consensus summarizer.
- [x] Mobile-friendly PWA interface with persistent 6-tab navigation (`Trips · Map · Plan · Money · Chat · You`).

#### Out-of-Scope (Explicit Anti-Features):
- ❌ **Commercial Flight & Hotel Booking APIs:** No live ticket checkouts or flaky booking APIs. Bookings are saved as locked anchor stops.
- ❌ **Unverified Web Auto-Scrapers:** The app never pulls unverified external restaurant lists; it only schedules places you and your friends actually pinned.
- ❌ **Live Currency Speculation:** Expense splitting is based on actual logged receipts, not speculative live foreign exchange scrapers.
