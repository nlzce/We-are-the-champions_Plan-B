# Plan B by We Are The Champions

## Team
**We Are The Champions**

- Tan Poh Zhai
- Lee Wai Loong
- Yap Chun Hoong
- Yap Shern Yu

**Track:** Lifestyle · Planning an Escape
**Problem Statement:** Travel Planner
**Submission:** Plan B
**Video file / YouTube title:** `We Are The Champions — Plan B — Planning an Escape`
**Video Presentation:** [Unlisted Youtube Link]
**Presentation Slides:** [Public Link]

---

## 1. Project Overview

### The Problem
Trip planning today is scattered across multiple disconnected tools: flight tickets stay buried in email, shared expenses get logged in Splitwise days late, wishlists live in Apple Notes, and real-time debates get lost in group chats.

This fragmentation leads to three major issues for group travel:
1. **Unstructured Preferences Lost in Chat:** Availability dates, personal budgets, and deal-breakers are discussed informally in group messaging. Without a structured way to normalize and intersect them, groups often compromise upward into overspending or settle on dates that do not work for everyone.
2. **Unconstrained AI & Brittle Booking APIs:** Generic travel chatbots frequently hallucinate fictional restaurants, closed attractions, and inaccurate prices, while commercial flight and hotel booking APIs introduce strict rate limits, checkout failures, and demo fragility.
3. **Single-Point Fragility During the Trip:** When a flight is delayed or an afternoon cafe is closed, existing apps force travelers to manually rearrange their entire multi-day schedule, which can easily cascade into missed hotel check-ins or ruined plans.

**Stakeholders:** University students, young working adults, and small friend groups (traveling solo or in groups of 2–6) looking for a phone-first tool to coordinate weekend getaways and holiday trips.

**Existing Apps & Why They Fall Short:**
- **Wanderlog / TripIt:** Geared toward individual business travelers or heavy itineraries built by parsing email receipts. They feel bloated on mobile, lock essential collaboration features behind paid subscriptions, ignore individual budget ceilings, and provide no automated way to patch a single disrupted hour on the fly.
- **Splitwise:** Strictly a retroactive expense tracker after money is spent. It has no connection to daily itinerary slots, meaning groups cannot enforce an upfront budget ceiling before overspending happens.
- **Google Maps Lists:** Helpful for bookmarking places, but completely static. They cannot calculate overlapping dates, sequence stops by travel pace, or dynamically adjust an afternoon schedule when unexpected delays occur.

### Our Solution
Plan B is a phone-first Progressive Web App (PWA) and grounded AI assistant designed to coordinate travelers from initial preference alignment to post-trip expense settlement. It gathers individual preferences, locks the group budget ceiling to the lowest personal budget so no one is priced out, and displays an open-source map powered by OpenStreetMap and Photon geocoding without external booking dependencies. During the trip, its dedicated Trip Mode tracks stops with live arrival countdowns; if a venue is delayed or closed, Plan B replaces only that specific time slot while keeping booked flights and hotel reservations permanently locked.

#### Core Feature-Set:
1. **Clean Identity & Rooms:** Lightweight authentication via Supabase Auth. Solo or group trip rooms can start completely empty without preloaded mock data.
2. **4-Step Preference Intake:** Gathers destination wishlist (with optional privacy toggle), travel date windows, personal budget limits with pace preferences, and interests with deal-breakers.
3. **Hidden Destination Privacy Toggle:** Allows a traveler to keep a destination suggestion private from peers for surprise trips. It never drops a pin on the shared map; only the trip's Gemini agent reads it server-side to build the route without revealing its name.
4. **Automated Alignment Engine:** Automatically calculates overlapping date windows, aggregates deal-breakers, locks the group budget ceiling to the lowest member cap (`min(individual caps)`), and requires destination confirmation before itinerary generation unlocks.
5. **Grounded Dual-Mode Map:** Displays only public, member-added places. Travelers can search places with fast Photon fuzzy autocomplete or tap directly on the map to pin custom coordinates.
6. **Constrained Gemini Agent:** Scoped strictly to a single trip room. Generates day-by-day itineraries using existing place IDs only; immediately halts if zero places exist, preventing hallucinated venues or fake prices.
7. **Dual Operating Modes:** Clear separation between **Plan Mode** (pre-trip alignment & schedule generation) and **Trip Mode** (live on-the-ground tracking & next-stop countdowns).
8. **Single-Slot Replanning (Core Innovation):** Marking a stop as delayed or skipped prompts Gemini to replace *only* that specific time slot using available places, while booked hotel stays and flights remain untouched.
9. **Itinerary-Tied Debt Ledger:** Expenses are logged directly against specific itinerary stops, automatically generating a minimal-transaction "who owes whom" debt graph.
10. **Real-Time Team Group Chat with Conversational AI Concierge & Opinion Summarizer:** In-room messaging powered by Supabase Realtime where members can chat, share interactive place cards, receive automatic broadcast alerts for replanned stops, and mention `@PlanB` to query trip parameters, settle debts, or synthesize unstructured debate into a structured consensus briefing with one-tap schedule updates.
11. **Mobile PWA Interface:** Built as a phone-first Progressive Web App (PWA) with a persistent 6-tab bottom navigation bar (`Trips · Map · Plan · Money · You · Chat`), optimized for quick one-handed mobile interactions.

#### The Core PWA Journey:
`Register / Login → Create trip or join with code → Individual members fill preferences (with optional hidden destination) & align → Confirm destination → Map pins + Itinerary + AI Agent sequences real pins → Team group chat coordination → Money expense logging (even / custom split) → Live Trip Mode (Done / Delay) → Single-slot Replan.`

---

## 2. Ideation & Process

### 2.1 Ideas We Considered

| Idea | Status | Why it was Kept / Dropped |
| :--- | :--- | :--- |
| **A. Plan B Grounded Collaborative Workspace (Chosen)** | **Kept (Chosen)** | Solves the core group coordination problem by replacing fragmented tools with a single trusted record for people, money, and days. The constrained Gemini agent only sequences places added by members, eliminating AI hallucinations and brittle booking API dependencies while delivering single-slot replanning. |
| **B. Vercel Serverless + Supabase Cloud Architecture (Chosen)** | **Kept (Chosen)** | Provides native Nuxt 3 (Nitro) serverless execution, PostgreSQL Row-Level Security (RLS) to enforce hidden destination privacy, and managed Realtime WebSockets for instant in-room group chat and live trip updates without maintaining dedicated websocket servers. |
| **C. Grounded Dual-Mode Map: Photon Search + Tap-to-Pin (Chosen)** | **Kept (Chosen)** | Combines open-source Photon geocoding (OpenStreetMap data) for fast, free-text fuzzy search autocomplete with direct map canvas tapping for custom coordinates. Delivers a reliable, responsive mapping experience without commercial API keys or vendor lock-in. |
| **D. Real-time Team Group Chat with In-Room AI Concierge (Chosen)** | **Kept (Chosen)** | Keeps communication and coordination directly inside the trip room via Supabase Realtime. Eliminates switching to external messaging apps by allowing members to share place cards, receive automated disruption alerts, and mention `@PlanB` to get instant group-visible answers on schedule, debts, or pacing without burdening the trip host. |
| **E. All-in-One Booking Super-App with Live Flight/Hotel APIs** | **Dropped** | Commercial booking APIs introduce rigid rate limits, high quota costs, and frequent checkout failures. Group travelers typically book flights and accommodations separately outside the app anyway; embedding ticket checkouts adds fake availability and unnecessary fragility. |
| **F. Conversational Free-Form Travel Chatbot** | **Dropped** | Unconstrained LLMs tend to generate generic tourist schedules with non-existent venue names, inaccurate pricing, and zero awareness of real group constraints or lowest-budget limits. |
| **G. Strict Public Nominatim Geocoder** | **Dropped** | Public Nominatim enforces a strict 1 request per second fair-use limit that breaks smooth search-as-you-type autocomplete, and often struggles with informal local venue names. Replaced with Photon. |

---

### 2.2 Ideation Boards & System Architecture Diagrams

All system workflows and domain models are embedded natively in Mermaid for direct GitHub rendering and slide presentations.

#### 2.2.1 Problem Tree
**Logic:** Three root organizational failures combine into one core breakdown: **the lack of a single trusted trip record**. This produces four major downstream failure modes: overspending, unaligned dates, hallucinated venues, and trip collapse after a minor transit delay.

```mermaid
flowchart TB
  subgraph CAUSES["Root Causes (How Work is Organised)"]
    C1["Fragmentation Across Apps<br/>Tickets in email, money in Splitwise,<br/>day lists in Notes, talk in WhatsApp"]
    C2["Unstructured Preferences in Chat<br/>Dates, budgets, and deal-breakers<br/>are spoken, never normalized or merged"]
    C3["Unconstrained AI & Dynamic APIs<br/>Models invent fake venues/prices;<br/>booking APIs introduce rate limits and failures"]
  end

  P["CORE PROBLEM<br/>The group has no single trusted trip record"]

  subgraph EFFECTS["Observed Downstream Effects"]
    E1["Dates and Budgets Never Lock<br/>Group compromises upward into overspending"]
    E2["Awkward Post-Trip Split Friction<br/>Spend tracked retroactively in separate tools"]
    E3["Hallucinated Itineraries<br/>AI schedules venues that do not exist in reality"]
    E4["Single-Point Fragility<br/>One delay collapses the whole day with no slot-level patch"]
  end

  C1 --> P
  C2 --> P
  C3 --> P
  P --> E1
  P --> E2
  P --> E3
  P --> E4
```

| Layer | Component | Deep Rationale |
| :--- | :--- | :--- |
| **Root Cause** | Fragmentation across apps | Bookings, shared money, and daily agendas never live in one shared state, forcing manual human synchronization. |
| **Root Cause** | Preferences lost in chat | Availability windows, budget ceilings, and hard deal-breakers are discussed informally, making automated alignment impossible. |
| **Root Cause** | Unconstrained AI & fragile APIs | Allowing an LLM to invent venues or relying on live booking APIs creates broken schedules and demo fragility. |
| **Core Problem** | **No single trusted trip record** | No participant can point to one authoritative ground truth for people, dates, budget ceiling, map pins, and schedule rows. |
| **Effect** | Endless negotiation & overspend | Without a computed group ceiling (minimum individual cap), groups default to the highest spender's wishes. |
| **Effect** | Post-trip settlement friction | Disconnected receipts produce awkward calculations days after returning home. |
| **Effect** | Fake shops and hallucinated venues | Itineraries look full on paper but fail on the ground. |
| **Effect** | Domino-effect trip failure | A missed train or closed cafe breaks the entire remaining schedule because there is no mechanism to patch just that hour. |

---

#### 2.2.2 System Boundary Use Case Diagram
**Logic:** Formal UML representation of the Plan B PWA workspace boundary, distinguishing interactions between **Travelers**, the **Trip Owner**, and the autonomous, trip-scoped **Gemini Agent**.

```mermaid
flowchart LR
  classDef actor fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,font-weight:bold;
  classDef usecase fill:#ffffff,stroke:#37474f,stroke-width:1.5px;
  classDef system fill:#f8f9fa,stroke:#455a64,stroke-width:2px;
  classDef ai fill:#ede7f6,stroke:#7e57c2,stroke-width:2px,font-weight:bold;

  Traveler["👤 Traveler / Member"]:::actor
  Owner["👑 Trip Owner"]:::actor
  Gemini["🤖 Trip-Scoped Gemini Agent"]:::ai

  Owner -.->|inherits| Traveler

  subgraph Boundary["System Boundary: Plan B PWA Workspace"]
    subgraph UC_Auth["1. Account & Profile"]
      UC01(["UC01: Register & Login<br/>(Supabase Auth)"]):::usecase
      UC02(["UC02: Manage Profile<br/>(Default pace, dietary, friends)"]):::usecase
    end

    subgraph UC_Room["2. Trip Rooms & Collaboration"]
      UC03(["UC03: Create / View Trip Room<br/>(Allowed to be empty initially)"]):::usecase
      UC04(["UC04: Join via Share Link<br/>(No prior friending required)"]):::usecase
      UC05(["UC05: Invite from Friends List"]):::usecase
      UC18(["UC18: Team Chat & @PlanB Concierge<br/>(Chat, place cards, opinion & consensus summary)"]):::usecase
    end

    subgraph UC_Prefs["3. Preferences & Display Map"]
      UC06(["UC06: Submit Preference Form<br/>(Where, dates, budget, pace, deal-breakers)"]):::usecase
      UC07(["UC07: Set Hidden Destination<br/>(Hidden from peers & map pins)"]):::usecase
      UC08(["UC08: Add Place via Search or Map Tap<br/>(Photon autocomplete or tap pin; open-source OSM)"]):::usecase
    end

    subgraph UC_Plan["4. Alignment & Scheduling"]
      UC09(["UC09: View Group Alignment<br/>(Date overlap, lowest cap ceiling)"]):::usecase
      UC10(["UC10: Confirm Destination"]):::usecase
      UC11(["UC11: Generate Day-by-Day Itinerary<br/>(Arrange existing place IDs only)"]):::usecase
      UC12(["UC12: Enforce Hallucination Guardrail<br/>(Halt if zero places exist)"]):::usecase
    end

    subgraph UC_Trip["5. Trip Mode & Slot Replan"]
      UC13(["UC13: View Next Stop"]):::usecase
      UC14(["UC14: Mark Delay / Cannot-Go"]):::usecase
      UC15(["UC15: Replan Single Disrupted Slot<br/>(Preserve locked hotels/flights)"]):::usecase
    end

    subgraph UC_Money["6. Expense & Settlement"]
      UC16(["UC16: Log Itemized Expense"]):::usecase
      UC17(["UC17: Settle Who-Owes-Whom<br/>(Graph debt minimization)"]):::usecase
    end
  end

  Traveler --- UC01
  Traveler --- UC02
  Traveler --- UC03
  Traveler --- UC04
  Traveler --- UC05
  Traveler --- UC06
  Traveler --- UC07
  Traveler --- UC08
  Traveler --- UC09
  Traveler --- UC13
  Traveler --- UC14
  Traveler --- UC16
  Traveler --- UC17
  Traveler --- UC18

  Owner --- UC10

  UC07 -.->|passes secretly to| Gemini
  UC11 -.->|requires| UC10
  UC11 --- Gemini
  UC12 --- Gemini
  UC14 -.->|triggers| UC15
  UC15 --- Gemini
  UC18 -.->|mentions @PlanB| Gemini
```

| Use Case ID | Name | Actor(s) | Preconditions & Business Rules |
| :--- | :--- | :--- | :--- |
| **UC01** | Register & Login | Traveler | Clean user authentication via Supabase Auth. |
| **UC02** | Manage Profile | Traveler | Sets personal defaults for travel pace, dietary requirements, and friend connections. |
| **UC03** | Create / View Room | Traveler | Can create solo trips or group rooms. Trips list is allowed to be empty initially. |
| **UC04** | Join via Share Link | Traveler | Anyone with the link or code can join; mutual friending is explicitly not required. |
| **UC05** | Invite from Friends | Traveler | Invites travelers directly from the user's friend connection list. |
| **UC06** | Submit Preference Form | Traveler | 4-step wizard: destination, date window, budget ceiling with pace, interests and deal-breakers. |
| **UC07** | Set Hidden Destination | Traveler | Destination toggle hides the place from peers and map display. Only Gemini reads it server-side. |
| **UC08** | Add Place (Search / Tap) | Traveler | Search places with Photon fuzzy autocomplete (OSM) or tap map to drop custom pin without external booking dependencies. |
| **UC09** | View Group Alignment | Traveler | Displays computed date overlaps, the group budget ceiling (`min(individual caps)`), and deal-breakers. |
| **UC10** | Confirm Destination | Trip Owner / Group | Prerequisite: Destination must be confirmed by the group before daily itinerary generation unlocks. |
| **UC11** | Generate Itinerary | Gemini Agent | Sequences member-added place IDs into days. Gemini is forbidden from inventing place names or prices. |
| **UC12** | Enforce Guardrail | Gemini Agent | If zero member-added places exist, agent halts immediately and prompts members to add places. |
| **UC13** | View Next Stop | Traveler | Displays current stop, arrival time, and live countdown in Trip Mode. |
| **UC14** | Mark Delay / Cannot-Go | Traveler | Flags a specific itinerary slot as disrupted during transit. |
| **UC15** | Replan Single Slot | Gemini Agent | Rewrites only the affected time block using remaining valid place IDs. Locked stays and flights remain fixed. |
| **UC16** | Log Itemized Expense | Traveler | Logs actual spend tied directly to an itinerary item with even or custom splitting. |
| **UC17** | Settle Who-Owes-Whom | Traveler | Computes debt graph and minimal transfers via Balance Equalizer without dynamic ticket lookup. |
| **UC18** | Team Chat & @PlanB Concierge | Traveler & Gemini Agent | Real-time in-room chat via Supabase Realtime; share place cards, receive live alerts, and invoke @PlanB to summarize group chat opinions, extract consensus, and query trip state. |

---

#### 2.2.3 End-to-End Activity Diagram
**Logic:** Swimlane activity diagram mapping the complete journey across **Traveler**, **System Engine**, **Trip-Scoped Gemini Agent**, and **Trip Execution/Money**.

```mermaid
flowchart TD
  subgraph MEMBER["👤 Traveler / Trip Member"]
    Start(["Start"]) --> Auth["Login / Register<br/>(Supabase Auth)"]
    Auth --> Profile["Update Profile Defaults<br/>(Pace, Dietary, Friends list)"]
    Profile --> RoomAction{"Create or Join Trip?"}
    RoomAction -->|Create Solo or Group| Create["Create Empty Trip Room<br/>('Penang with the gang')"]
    RoomAction -->|Join via Share Code / Link| JoinLink["Join Trip via Code: 688422<br/>(No prior friendship required)"]
    RoomAction -->|Join via Friend Invite| JoinFriend["Accept Friend Invite"]

    Create --> FillPrefs["Submit 4-Step Preference Form<br/>(Destination, dates, budget cap,<br/>pace, interests, deal-breakers)"]
    JoinLink --> FillPrefs
    JoinFriend --> FillPrefs

    FillPrefs --> HideToggle{"Hide Destination?"}
    HideToggle -->|Yes: Secret Wishlist| MarkHidden["Toggle 'Hide from group'<br/>(Omitted from shared map pins)"]
    HideToggle -->|No: Public Suggestion| MarkPublic["Keep Destination Public"]

    MarkHidden --> AddPlace["Add Places: Search via Photon<br/>OR Tap Map for Custom Coordinates"]
    MarkPublic --> AddPlace

    AddPlace --> CheckAlign["Review Group Alignment Dashboard<br/>(Alex, Jamie, Sam, Riley)"]
    CheckAlign --> ChatDebate["Team Group Chat Discussion<br/>(Coordinate stops, pace & budgets)"]
    ChatDebate --> MentionBot["Invoke '@PlanB summarize opinions'<br/>(Extract consensus & conflicts)"]
  end

  subgraph SYSTEM["⚙️ Plan B System Engine"]
    CheckAlign --> RunAlign["Compute Alignment Metrics:<br/>1. Find date overlap windows<br/>2. Budget ceiling = lowest individual cap<br/>3. Aggregate shared deal-breakers"]
    RunAlign --> DestConfirm{"Destination Confirmed<br/>by Group?"}
    DestConfirm -->|No| CheckAlign
    DestConfirm -->|Yes: 'Penang, Malaysia'| ValidatePlaces{"Member-Added Place IDs > 0?"}
  end

  subgraph GEMINI["🤖 Trip-Scoped Gemini Agent"]
    MentionBot --> ExtractConsensus["Extract Chat Consensus & Opinions:<br/>- Agreed stops (Tek Sen dinner)<br/>- Pace desires (Sam's relaxed Day 2)<br/>- Open items (Riley budget check)"]
    ExtractConsensus --> PostSummary["Broadcast Consensus Card to Room<br/>(One-tap apply to itinerary)"]
    PostSummary -.-> DestConfirm

    ValidatePlaces -->|No| Halt["STOP SCHEDULING<br/>(Zero fake venues or prices invented.<br/>Prompts members to add places)"]
    Halt -.-> AddPlace

    ValidatePlaces -->|Yes| IngestData["Ingest Trip Data Server-Side:<br/>- Allowed place IDs<br/>- Hidden destination (Secretly)<br/>- Group budget cap & Pace<br/>- Applied consensus agreements"]
    IngestData --> ScheduleDays["Generate Day-by-Day Itinerary<br/>(Restricted to existing place IDs;<br/>NEVER reveals hidden destination name)"]
  end

  subgraph TRIP_MONEY["🚀 Trip Execution & Expense Settlement"]
    ScheduleDays --> SwitchTrip["Switch to Trip Mode"]
    SwitchTrip --> NextStop["Display Next Stop & Countdown<br/>(Kek Lok Si Temple · 09:00)"]

    NextStop --> EventCheck{"Trip Status"}
    EventCheck -->|Normal Progress| LogSpend["Log Expense on Itinerary Item<br/>(Even or Custom Split)"]
    EventCheck -->|Disruption Occurs| MarkIssue["Mark Slot: Delay / Cannot Go<br/>(e.g. Heavy rain at Air Itam)"]

    MarkIssue --> LockTrip["Lock Remaining Itinerary:<br/>Flights & booked stays untouched"]
    LockTrip --> ReplanSlot["Gemini: Replan ONLY Disrupted Slot<br/>using available valid place IDs"]
    ReplanSlot --> NextStop

    LogSpend --> DebtCalc["Calculate Debt Graph:<br/>Balance Equalizer who-owes-whom"]
    DebtCalc --> EndTrip(["Trip Complete & Settled"])
  end
```

---

#### 2.2.4 Dedicated Functional Architecture Diagrams

##### Function 1: Automated Alignment & Destination Lock Engine
**Description:** Calculates the overlapping availability window, locks the group budget ceiling strictly to the lowest individual cap, aggregates deal-breaker constraints, and enforces a mandatory destination confirmation lock before itinerary generation unlocks.

```mermaid
flowchart TD
  M1["Alex (Host) Form<br/>Dates: Oct 10-15 · Cap: RM 2000 · Pace: Easy"] --> Engine["Alignment Calculation Engine"]
  M2["Jamie Form<br/>Dates: Oct 10-15 · Cap: RM 800 · Pace: Balanced"] --> Engine
  M3["Riley Form<br/>Dates: Oct 10-14 · Cap: RM 450 · Pace: Relaxed"] --> Engine

  Engine --> CalcDate["Calculate Date Intersection<br/>Overlap Window: Oct 10 - 14 (5 Days)"]
  Engine --> CalcCap["Calculate Group Budget Ceiling<br/>min(2000, 800, 450) = RM 450 / person<br/>(Lowest cap protects lowest spender)"]
  Engine --> CalcBreakers["Aggregate Deal-Breakers<br/>Merged: No 4am starts · No heavy museums"]

  CalcDate --> AlignCard["Render Group Alignment Summary<br/>(Date Overlap, Cap RM 450, Prefs in: 3/4)"]
  CalcCap --> AlignCard
  CalcBreakers --> AlignCard

  AlignCard --> LockCheck{"Group Confirmed<br/>Destination?"}
  LockCheck -->|No| Pending["Display Lock Badge: Pending Confirmation<br/>Itinerary generation remains locked"]
  LockCheck -->|Yes: Penang, Malaysia| Locked["Display Lock Badge: Destination Confirmed ✓<br/>Unlock Itinerary Generation Button"]
```

---

##### Function 2: Hidden Destination Privacy & Server-Side Shielding
**Description:** Ensures private surprise wishlists are never leaked to peers via UI or map pins. Client queries are blocked by PostgreSQL Row-Level Security (RLS), while Gemini processes the destination server-side under a strict system prompt forbidding it from ever revealing the name.

```mermaid
flowchart TD
  User["Traveler (Alex)"] --> InputDest["Input Destination: 'Kyoto'"]
  InputDest --> Toggle["Toggle: [ Hide from group (Only AI uses this for planning) ]"]

  Toggle --> ClientUI["Client UI State (Alex)"]
  Toggle --> ServerAPI["Server API: /api/preferences/submit"]

  ServerAPI --> DB[(Supabase PostgreSQL)]
  DB --> RLS["Row-Level Security (RLS) Policy<br/>is_hidden = true"]

  RLS -->|Peers: Jamie, Sam, Riley| BlockPeers["Map Query: Excluded from pins<br/>Peers cannot see coordinate or venue name"]
  RLS -->|Member: Alex| AllowOwner["Alex sees private indicator badge"]

  DB --> ServerGemini["Trip-Scoped Gemini Server Route<br/>(Reads is_hidden destination securely)"]
  ServerGemini --> Prompt["System Instruction Guardrail:<br/>'Factor in private destination characteristics,<br/>but NEVER state its name in outputs.'"]
  Prompt --> ItineraryOut["Generated Itinerary Response<br/>(Secretly incorporates slot without leaking venue)"]
```

---

##### Function 3: Grounded Dual-Mode Map Engine
**Description:** Open-source map integration without commercial API dependencies. Combines real-time Photon fuzzy search autocomplete on OpenStreetMap data with direct canvas tapping for arbitrary custom coordinates.

```mermaid
flowchart TD
  UserAction{"How Traveler Adds Place"}

  UserAction -->|Type in Search Bar| SearchInput["Search Query: 'Tek Sen Restaurant'"]
  SearchInput --> PhotonAPI["Photon Fuzzy Geocoder API<br/>(OpenStreetMap by Komoot)"]
  PhotonAPI --> AutocompleteList["Dropdown Autocomplete List<br/>Real venues with address & district"]
  AutocompleteList --> SelectResult["Traveler selects search result"]
  SelectResult --> ExtractCoords["Extract lat/lng & place metadata"]

  UserAction -->|Tap on Map Canvas| MapClick["Leaflet click event on OSM tiles"]
  MapClick --> ClickCoords["Capture clicked lat/lng coordinates"]
  ClickCoords --> PromptName["Prompt traveler for venue label & tag"]
  PromptName --> ExtractCoords

  ExtractCoords --> SaveDB["Write to Supabase: trip_places table<br/>(trip_id, name, lat, lng, added_by)"]
  SaveDB --> RenderMap["Render Numbered Pin on Map<br/>(Pin 1, Pin 2, Pin 3...)"]
  RenderMap --> Drawer["Open Bottom Drawer Sheet:<br/>Place Details + '+ Add to Itinerary Pool'"]
```

---

##### Function 4: Constrained Gemini Planner & 0-Place Guardrail
**Description:** Enforces strict grounding. The Gemini model is constrained to an allow-list of member-added place IDs. If zero places exist, the engine halts immediately and refuses to fabricate fictional restaurants or attractions.

```mermaid
flowchart TD
  Trigger["User triggers: 'Generate Itinerary'"] --> FetchPlaces["Fetch trip_places from DB where trip_id = current"]
  FetchPlaces --> CountCheck{"Count(trip_places) > 0?"}

  CountCheck -->|No: Count == 0| HaltBranch["🛑 HARD STOP GUARDRAIL TRIGGERED<br/>Zero places exist in trip pool"]
  HaltBranch --> HaltResponse["Return UI Warning:<br/>'Cannot generate schedule: 0 places in pool.<br/>Please add places via Map or Form first.'<br/>(Zero invented venues or fake prices)"]

  CountCheck -->|Yes: Count > 0| PreparePayload["Prepare Grounded Agent Payload:<br/>- Allowed Place IDs: [Tek Sen, Kek Lok Si, Penang Hill...]<br/>- Group Budget Cap: RM 450<br/>- Group Pace: Easy"]
  PreparePayload --> GeminiCall["Call Google Gemini API<br/>(Enforce JSON Mode & Place ID Schema)"]
  GeminiCall --> ValidateJSON["Server-side JSON Schema Validation:<br/>Verify all returned IDs exist in allow-list"]
  ValidateJSON --> WriteDays["Write rows to trip_itinerary table<br/>(Day 1, Day 2 with verified IDs)"]
```

---

##### Function 5: Live Trip Mode & Surgical Single-Slot Replan
**Description:** The core product differentiator. In Trip Mode, when a delay or closure occurs, the system preserves all locked flights and hotel stays while instructing Gemini to recalculate only the single disrupted time block.

```mermaid
flowchart TD
  TripMode["Live Trip Mode Active (Penang)<br/>NEXT STOP: Kek Lok Si Temple (09:00)"] --> Disruption{"Disruption Occurs<br/>on active slot (09:00 AM)"}

  Disruption -->|Member taps Delay| DelayBtn["⚠️ Delayed (+30m)<br/>Reason: Heavy rain at Air Itam"]
  Disruption -->|Member taps Cannot Go| SkipBtn["❌ Cannot Go / Closed"]

  DelayBtn --> ReplanTrigger["Trigger Single-Slot Replan Engine"]
  SkipBtn --> ReplanTrigger

  ReplanTrigger --> LockStays["🔒 LOCK INTEGRITY ENFORCEMENT:<br/>- Booked Hotel Check-in: LOCKED<br/>- Return Flights: LOCKED<br/>(Never shifted or modified)"]

  LockStays --> QueryPool["Query unused places from trip_places pool"]
  QueryPool --> GeminiReplan["Gemini: Replan ONLY Disrupted 09:00 Slot<br/>Constraints: Fits into morning window<br/>Remaining budget under RM 450 cap"]

  GeminiReplan --> ReturnOptions["Generate Replacement Candidates:<br/>1. Penang State Museum (Indoor, free entry)<br/>2. ChinaHouse Cafe (Indoor heritage cafe)<br/>3. Cheong Fatt Tze Blue Mansion"]

  ReturnOptions --> ReplanModal["Display Mid-Trip Replan Modal<br/>(Highlights delayed slot in yellow,<br/>locked stays unchanged)"]
  ReplanModal --> UserConfirm["User selects candidate & applies replan"]
  UserConfirm --> PatchSlot["Update ONLY that single slot in DB<br/>Resume Live Trip Mode"]
```

---

##### Function 6: Who-Owes-Whom Debt Graph Settlement Engine
**Description:** Itemized receipts tied directly to itinerary items are parsed by a graph minimization algorithm that computes the minimal number of peer-to-peer transfers required to settle all debts.

```mermaid
flowchart TD
  Log1["Expense 1: RM 240 Rooftop Drinks<br/>Paid by Alex · Custom Split (Riley RM 0)"] --> Ledger["Trip Expense Ledger<br/>TOTAL SPENT RM 368.50"]
  Log2["Expense 2: RM 128.50 Grab Rides<br/>Paid by Jamie · Even Split 4 ways"] --> Ledger

  Ledger --> NetCalc["Calculate Net Balance for Each Member:<br/>Net = Total Paid - Fair Share"]

  NetCalc --> B1["Alex: Paid RM 240.00 ➔ Net: +RM 147.87 (Gets)"]
  NetCalc --> B2["Jamie: Paid RM 128.50 ➔ Net: +RM 36.37 (Gets)"]
  NetCalc --> B3["Sam: Paid RM 0.00 ➔ Net: -RM 92.12 (Owes)"]
  NetCalc --> B4["Riley: Paid RM 0.00 ➔ Net: -RM 92.12 (Owes)"]

  B1 --> GraphSolve["Greedy Debt Simplification Algorithm<br/>(Minimizes total transaction count)"]
  B2 --> GraphSolve
  B3 --> GraphSolve
  B4 --> GraphSolve

  GraphSolve --> Settle1["Transaction 1: Sam pays Alex RM 92.12"]
  GraphSolve --> Settle2["Transaction 2: Riley pays Alex RM 55.75"]
  GraphSolve --> Settle3["Transaction 3: Riley pays Jamie RM 36.37"]

  Settle1 --> UI["Render Balance Equalizer UI Cards<br/>with '[ Mark Paid ]' Buttons"]
  Settle2 --> UI
  Settle3 --> UI
```

---

##### Function 7: Real-Time Team Group Chat with In-Room AI Concierge & Consensus Summarizer
**Description:** Powers intra-room communication via Supabase Realtime websocket channels. Combines peer-to-peer messaging, place card sharing, and automated broadcast alerts with an in-room Gemini concierge (`@PlanB`). When invoked, the AI analyzes conversation history and member preferences to extract a structured consensus summary (resolving pacing or dining debates) and renders an actionable schedule diff card directly into the chat feed.

```mermaid
flowchart TD
  subgraph ROOM_CLIENTS["Connected Trip Room Members (Mobile PWA)"]
    ClientA["Alex (Host)"]
    ClientB["Jamie"]
    ClientC["Sam"]
    ClientD["Riley"]
  end

  subgraph SUPABASE_RT["Supabase Realtime Engine (Channel: 'trip_room:penang')"]
    Broadcaster["WebSocket Broadcast Hub"]
  end

  subgraph EVENTS["Event Triggers inside Trip Room"]
    HumanMsg["Jamie: 'I pinned Tek Sen on the map!'"]
    SamMsg["Sam: 'Can we keep Day 2 afternoon relaxed?'"]
    RileyMsg["Riley: 'My budget cap is RM 450/pax'"]
    MentionMsg["Alex: '@PlanB summarize our opinions'"]
    ReplanAlert["⚡ System Alert: 'Kek Lok Si delayed due to rain; replanned'"]
    SpendAlert["💵 System Alert: 'Alex logged RM 240 Rooftop drinks'"]
  end

  subgraph AI_CONCIERGE["🤖 Plan B In-Room AI Concierge"]
    DetectMention{"Message contains<br/>@PlanB mention?"}
    SummarizeChat["Analyze Chat History & Prefs:<br/>1. Consensus: Tek Sen & relaxed afternoon<br/>2. Open conflict: Day 2 dinner budget<br/>3. Generate structured diff card"]
    ReplyBroadcast["Broadcast AI Consensus Card to Room"]
  end

  HumanMsg --> Broadcaster
  SamMsg --> Broadcaster
  RileyMsg --> Broadcaster
  MentionMsg --> Broadcaster
  ReplanAlert --> Broadcaster
  SpendAlert --> Broadcaster

  Broadcaster --> DetectMention
  DetectMention -->|Yes: @PlanB| SummarizeChat
  SummarizeChat --> ReplyBroadcast
  ReplyBroadcast --> Broadcaster

  Broadcaster <--> ClientA
  Broadcaster <--> ClientB
  Broadcaster <--> ClientC
  Broadcaster <--> ClientD

  ClientA --> ChatUI["Team Group Chat Tab:<br/>- Text bubbles with timestamps<br/>- Interactive place cards with map links<br/>- System event notice pills<br/>- AI Consensus Summary Diff Cards"]
  ClientB --> ChatUI
  ClientC --> ChatUI
  ClientD --> ChatUI
```

---

#### 2.2.5 14-Screen End-to-End User Flow
**Logic:** Strict sequential flow across all screens. Users cannot trigger Gemini generation until destination is confirmed and real places exist.

```mermaid
flowchart TD
  S01["01 Splash<br/>Brand & Slogan"] --> S02["02 Login / Register<br/>Supabase Auth"]
  S02 --> S03["03 Trips<br/>List / Join with Code"]
  S03 --> S04["04 Trip Home: Align<br/>Alex, Jamie, Sam, Riley"]
  S03 --> S17["17 Invite<br/>Code 688422 & Link"]
  S04 --> S05_08["05-08 Preferences 1-4<br/>Destination, Dates, Budget, Interests"]
  S05_08 --> S09["09 Align Filled<br/>Overlap Window & RM 450 Cap"]
  S09 --> DestCheck{"Confirm Destination?<br/>(Penang, Malaysia)"}
  DestCheck -->|No| S04
  DestCheck -->|Yes| S10["10 Trip Home: Plan<br/>4 Entry Cards"]
  S10 --> S11["11 Map<br/>Add & Pin Real Venues"]
  S10 --> S20_21["20-21 Group Chat<br/>Coordinate, @PlanB Consensus & Pins"]
  S20_21 -.->|Apply Consensus Diff| S14
  S11 --> PlaceCheck{"Places in Pool > 0?"}
  PlaceCheck -->|No| S11
  PlaceCheck -->|Yes| S14["14 AI Agent<br/>Gemini Sequences Places"]
  S14 --> S12_13["12-13 Itinerary<br/>Day 1 & Day 2 Real Stops"]
  S12_13 --> S18["18 Trip Mode<br/>NEXT STOP Kek Lok Si (09:00)"]
  S18 --> DisCheck{"Delay or Skip?<br/>(Heavy rain)"}
  DisCheck -->|Yes| S19["19 Mid-Trip Replan<br/>Patch Slot; Keep Stays Locked"]
  S19 --> S18
  DisCheck -->|No| S15["15-15b Money<br/>Total Spent RM 368.50 & Splits"]
  S03 --> S16["16 Profile: You<br/>Trip Count & Settings"]
```

Bottom navigation tabs (always accessible in PWA): **Trips · Map · Plan · Money · You · Chat**

---

#### 2.2.6 Idea Evolution
**Logic:** Each architectural pivot deliberately eliminated a **source of untrue data** and architectural complexity. Product guardrails (no fake pins, no invented shops, no checkout failures) are the direct outcome of this progression.

```mermaid
flowchart LR
  V1["V1: Map + Booking APIs"] --> V2["V2: Strict Nominatim (1 req/s)"]
  V2 --> V3["V3 FINAL: Plan B PWA<br/>Vercel + Supabase + OSM + Photon"]
```

| Architecture Pivot | What We Explored | Why It Failed / Was Discarded | Source of Untrue Data / Complexity Removed |
| :--- | :--- | :--- | :--- |
| **V1: Booking Super-App** | Integrated live flight, hotel, and attraction booking APIs | API keys fail, rate limits hit, and failure modes produce fake prices and broken demo flows. | In-app ticket checkout, dynamic live fares, fake availability counters. |
| **V2: Fragile Geocoding** | Free-text search geocoded via public Nominatim / Google Places | Public Nominatim enforces 1 req/s, fails on informal names, and drops pins in the wrong country. | Unreliable commercial geocoding APIs and misplaced coordinate pins. |
| **V3 (Final Production Stack): Plan B on Vercel + Supabase** | Nuxt 3 (Nitro) on Vercel Serverless + Supabase (PostgreSQL with RLS, Auth, Realtime CDC) + Leaflet OSM & Photon geocoder + Scoped Gemini Agent. | 100% buildable, native Vue SSR hydration, battle-tested PostgreSQL RLS isolation, instant managed websockets for group chat, fast fuzzy OSM autocomplete, and 0-place halt guardrail. | Hallucinated shops, invented venue names, fake reviews, checkout API drop-offs, and state synchronization leaks. |

---

#### 2.2.7 Alternative Ideas Comparison
**Logic:** Evaluated three distinct paradigms for the hackathon brief. Idea C (Plan B Workspace) was selected because it directly solves the organizational gap without relying on fragile external data.

```mermaid
flowchart TB
  Q["Challenge: How to synchronize a group trip and adapt mid-way?"]
  Q --> A["A. Booking Super-App"]
  Q --> B["B. Conversational Itinerary Bot"]
  Q --> C["C. Plan B Workspace (Chosen)"]

  A --> A1["Strong on inventory<br/>Weak on group caps and slot replanning"]
  B --> B1["Fast day lists<br/>Invented fake shops; no shared ceiling"]
  C --> C1["Structured forms + tapped pins + constrained Gemini<br/>Truthful, resilient, slot-level replan"]
```

| Evaluation Criteria | A. Booking Super-App | B. Conversational Itinerary Bot | **C. Plan B Workspace (Chosen)** |
| :--- | :--- | :--- | :--- |
| **Core Job** | Purchase flights and hotel stays | Generate full travel text from a prompt | Maintain one trusted record, then rearrange it |
| **Data Source** | Live commercial booking APIs | LLM parametric weights | Member preference forms + tapped map pins |
| **Group Sync** | Poor (individual checkout only) | None (single-user chat session) | **Native** (Solo or group, join by code/link) |
| **Budget Handling** | Shows price per item; ignores caps | Ignores collective financial constraints | **Strict ceiling** (`min(individual caps)`) |
| **Disruption Replan** | Start inventory search over again | Regenerates entire multi-day prompt | **Patches only the single affected slot** |
| **Data Honesty** | Fragile API failure modes | High rate of hallucinated shops & places | **Zero hallucinations** (hard halt if 0 places) |
| **Hackathon Viability** | High quota cost, brittle live demo | Looks neat, breaks on inspection | **100% buildable, reliable, and truthful** |

---

#### 2.2.8 System Topology Mindmap

```mermaid
mindmap
  root((Plan B))
    Users and Collaboration
      Target audience
        University students
        Small friend groups
        Short phone-first trips
      Room lifecycle
        Solo trip room
        Group collaboration room
        Real-time team group chat
        Join by share code 688422 without friending
        Invite from friends list
        Trip list allowed empty initially
      Identity and profiles
        Clean registration and login
        Zero preloaded mock users
        Profile defaults
          Default travel pace
          Dietary restrictions
          Friends entry
    Preference Gathering
      Individual preference form
        Destination wishlist
        Available travel dates
        Personal budget ceiling
        Pace preference
        Specific interests
        Dislikes and deal-breakers
        Must-visit places
      Hidden destination feature
        Destination marked hidden from peers
        No pins dropped on shared map
        Ingested privately by Gemini server-side
        Gemini strictly forbidden to speak name
    Alignment and Scheduling
      Alignment calculation
        Date overlap window discovery
        Group budget ceiling equals lowest cap
        Shared deal-breaker aggregation
      Itinerary sequencing
        Confirm destination lock prerequisite
        Sequence days strictly using existing places
    Constrained Gemini Agent
      Trip-scoped architecture
        One dedicated agent per trip room
        Reads only this trip database rows
      Strict grounding guardrails
        Schedules existing place IDs only
        Halt execution if zero places exist
        Zero invented venue names
        Zero invented prices
    Dual Operating Modes
      Plan Mode
        Group preference synchronization
        Destination confirmation
        Day-by-Day itinerary generation
      Trip Mode
        Live countdown to next stop
        One-tap delay or cannot-go flag
        Slot-only replanning engine
          Patches only the disrupted hour
          Locked flights and stays remain fixed
    Map and Money Ledgers
      Grounded map engine
        Leaflet with OpenStreetMap tiles
        Photon open-source fuzzy search
        Tap map for custom coordinates
        Displays public pins only
        Open-source geocoding engine
        Empty inputs produce empty map
      Expense and split ledger
        Log actual spend on itinerary items
        Automated who-owes-whom debt graph
        Balance Equalizer net settlement
        No live dynamic ticket fare scraping
    PWA Interface and Screens
      PWA mobile architecture
        390px mobile phone width
        Six bottom navigation tabs
          Trips tab
          Map tab
          Plan tab
          Money tab
          You tab
          Chat tab
      Core screens
        01 Splash brand icon and slogan
        02 Login and Register
        03 Trips room list
        04 Trip Home alignment hub
        05-08 4-step Preference wizard
        09 Alignment summary
        10 Trip Home plan mode
        11 Map public pins
        12-13 Itinerary empty and filled
        14 Agent Gemini chat
        15-15b Money and custom split
        16 Profile defaults and stats
        17 Invite via code 688422
        18 Trip Mode live execution
        19 Mid-trip replan single slot
        20-21 Group Chat real-time messaging
    System Tech Stack
      Frontend Nuxt 3 and Vue 3
      Backend Nitro and Drizzle ORM
      Languages TypeScript and pnpm and Node
      Data Supabase Auth Postgres Realtime RLS
      AI Engine Google Gemini trip-scoped
      Maps Leaflet and OpenStreetMap tiles
      Hosting Vercel and Supabase
    Explicit Anti-Features
      No flight or hotel booking checkouts
      No auto nearby venue scraper
      No live dynamic fare lookup
```

##### Mindmap Pillar Summary

```text
Plan B System Topology
├── 1. Users & Collaboration: Students & friends | Solo or group | Real-time chat | Code join 688422 | Clean auth
├── 2. Preference Gathering: 4-step wizard | Hidden destination (no map pin, Gemini reads privately)
├── 3. Alignment & Scheduling: Date overlaps | Budget ceiling = min(caps) | Destination lock prerequisite
├── 4. Constrained Gemini Agent: Scoped to single trip | Existing place IDs only | Stop on 0 places | Zero fake shops
├── 5. Dual Operating Modes: Plan Mode (align/schedule) | Trip Mode (next stop, delay flag, single-slot replan, stay lock)
├── 6. Map & Money Ledgers: Photon search + tap-to-pin (open-source OSM) | Itemized spend | Balance Equalizer (even/custom)
├── 7. PWA Interface & Screens: 390px mobile layout | 6 bottom tabs (Trips, Map, Plan, Money, You, Chat) | 21 screens (01-21)
├── 8. System Tech Stack: Nuxt 3 + Vue 3 | Nitro Server | Drizzle ORM | Supabase (RLS, Realtime) | Gemini | Leaflet OSM | Vercel
└── 9. Explicit Anti-Features: No booking checkout APIs | No auto venue scraping | No dynamic fares
```

---

### 2.3 Mentor Consultation

| Date | Mentor | Feedback Received | What Was Changed |
| :--- | :--- | :--- | :--- |
| `[Session 1]` | `[Mentor Name]` | *Feedback on scoping, grounding, or feasibility will be recorded here.* | *Corresponding architectural or UI changes.* |
| `[Session 2]` | `[Mentor Name]` | *Feedback on presentation, user flow, or edge cases will be recorded here.* | *Corresponding refinements made to submission.* |

---

## 3. Design & Prototype

**UI Prototype:** [ Public Link ]

### Key Screens & User Interactions

Plan B is built as a phone-first Progressive Web App (PWA) with a persistent 6-tab bottom navigation bar (`Trips · Map · Plan · Money · You · Chat`), giving travelers instant access to every part of their trip on the move.

1. **Screen 01 & 02: Splash & Authentication**
   Clean cold-start brand screen with the Plan B slogan (*"When Plan A fails, Plan B saves the trip"*). The login screen highlights three core value propositions: **Group trips**, **AI-powered**, and **Split costs**, with instant account sign-up, email login, or Apple/Google sign-in.

2. **Screen 03 & 04: Trips List & Trip Home (Align Mode)**
   Shows the user's trips list (`Good morning, Alex`, `+ New Trip`, `Join with code`). Tapping into a trip opens the Trip Home on the **Align** tab (e.g., *"Penang with the gang"*, 4 members: Alex Host, Jamie, Sam, Riley), showing real-time preference submission statuses, an `+ Invite` button, and the destination lock prompt.

3. **Screen 05–08: 4-Step Preference Intake Form**
   - **Step 1 (Destination):** Allows members to propose a place (e.g., `Kyoto`) with a `Hide from group (Only AI uses this for planning)` toggle to protect surprise wishlists.
   - **Step 2 (Dates):** Captures individual travel windows (`Available from 10/10/2026 to 10/15/2026`) to compute group date overlaps.
   - **Step 3 (Budget & Pace):** Sets personal budget caps (e.g., `RM 2000`), explicitly noting that the group ceiling is capped to the lowest submitted amount, alongside preferred pace (`Slow` / `Easy` / `Fast`).
   - **Step 4 (Interests & Deal-Breakers):** Multi-select interest tags (`Culture`, `Food`, `Nature`, `Shopping`, `Nightlife`), explicit deal-breakers (`No 4am starts`), and must-dos.

4. **Screen 09 & 10: Alignment Summary & Trip Home (Plan Mode)**
   Once submitted, the member status turns `Ready`. The **Group Alignment** summary displays the computed date overlap, the group budget ceiling, and completion progress (`1/4 prefs in`). Once the destination is locked (`Penang, Malaysia`), switching to the **Plan** tab reveals four clear feature cards: `Itinerary`, `Map`, `AI Agent`, and `Budget`.

5. **Screen 11: Grounded Dual-Mode Map**
   Interactive Leaflet OpenStreetMap view where travelers search venues via Photon fuzzy autocomplete or tap directly on the map canvas to pin custom locations. Saved pins automatically become candidate stops for the trip pool—Gemini only sequences real pins and never invents locations.

6. **Screen 12 & 13: Grounded Day-by-Day Itinerary**
   Displays an empty state (`No activities yet` with Map and AI shortcuts) or a sequenced daily itinerary (Day 1: `Kek Lok Si Temple (09:00, 60min)`, `Char Koay Teow lunch (12:30)`; Day 2: `Penang Hill funicular (10:00)`). Each entry includes venue details, duration estimates, and locked badges for booked flights or accommodations.

7. **Screen 14: Trip-Scoped AI Agent (Trip Context Oracle & Itinerary Controller)**
   A dedicated itinerary control interface restricted strictly to the current trip room. A top context bar displays real-time trip parameters (`3 pins · 1/4 prefs · Budget cap: RM 450`). Rather than generic chit-chat, the agent serves as an intelligent itinerary controller grounded in the trip's live database: travelers can directly query trip logistics in plain text (e.g. *"What time do we leave tomorrow?"*, *"Who owes money right now?"*, *"Is dinner vegetarian-friendly for Jamie?"*) or trigger one-tap schedule mutations via quick chips (`Relax pace`, `Swap Day 2`, `Add meal stop`) without inventing fake venues or prices.

8. **Screen 15 & 15b: Money & Balance Equalizer**
   Transparent shared expense ledger showing `TOTAL SPENT RM 368.50` (`RM 92.13 / person`). The **Balance Equalizer** visualizes who paid, fair shares, and net balances (green `gets` / red `owes`). The `Add Expense` sheet supports both **Even Split** (e.g., `Grab rides`) and **Custom Split** (e.g., `Rooftop drinks`, where Riley is set to RM 0).

9. **Screen 16 & 17: Profile (You) & Invite**
   The Profile tab displays member stats (`1 Total Trips, 0 Active`) and account settings. The Invite screen provides a 6-digit room code (`688422`) and shareable link (`planb.app/join/688422`), allowing friends to join and submit preferences without forced pre-registration.

10. **Screen 18 & 19: Live Trip Mode & Mid-Trip Replan**
    On the day of travel, Trip Mode features a prominent `NEXT STOP` card (`Kek Lok Si Temple`) with `Done`, `Delay`, and `Skip` buttons. When a disruption occurs (e.g., `Heavy rain at Air Itam`), the **Mid-Trip Replan** modal recalculates *only* that affected time slot using remaining places in the pool, keeping all booked flights and hotel reservations intact.

11. **Screen 20 & 21: Real-Time Team Group Chat with Conversational AI Concierge & Consensus Summarizer**  
    An integrated in-room chat (accessible via the `Chat` tab) where members coordinate plans (Jamie pins `Tek Sen`, Sam asks for a relaxed afternoon, Riley shares a budget constraint). Beyond peer-to-peer messaging, the AI agent participates directly in the conversation as an in-room concierge: any traveler can mention `@PlanB` (e.g., *"@PlanB what time is hotel check-out?"*, *"@PlanB who owes Alex money?"*, or *"@PlanB make tomorrow morning more relaxed"*) to receive instant, group-visible answers and schedule diff cards. Crucially, when discussions grow long, members can prompt *"@PlanB summarize our opinions"* to synthesize chat history and member preferences into a structured consensus briefing (agreed stops, pacing desires, and open budget decisions) with a one-tap button to apply consensus directly to the trip itinerary. The chat also receives automatic broadcast notifications whenever a slot is replanned or an expense is logged.

---

## 4. What Makes It Different

| Evaluation Dimension | Traditional Travel Apps (Wanderlog, TripIt) | Group Chat + Splitwise | Generic Travel AI Bots | **Plan B (Our Solution)** |
| :--- | :--- | :--- | :--- | :--- |
| **Itinerary Construction** | Manual email parsing or static lists | None; fragmented text in notes | Unconstrained hallucinated venues & fake prices | **Sequenced strictly from real member-added place IDs** |
| **Group Preference Alignment** | Weak; assumes one person plans everything | Endless unstructured debates in chat | Single-user prompt; no group context | **4-step preference form + automated overlap engine** |
| **Budget Enforcement** | Passive cost display; ignores caps | Retroactive accounting after overspending | Ignores budgets or invents fake costs | **Upfront group ceiling locked to lowest individual cap** |
| **Surprise / Private Wishlists** | Non-existent; everything is public | Leaked immediately in group chat | N/A | **Hidden destination toggle (server-side Gemini isolation)** |
| **Mid-Trip Disruption Response** | Manual multi-day rescheduling | Panic in chat; manual reshuffling | Re-generates entire itinerary from scratch | **Single-slot replanning (preserves locked stays/flights)** |
| **Expense Settlement** | Requires paid tier or external app | Separate app disconnected from agenda | None | **In-room debt graph tied directly to itinerary items** |
| **Team Communication** | External (WhatsApp / Telegram) | Fragmented across chat history | Single-player chatbot session | **In-room group chat with @PlanB AI bot & live itinerary broadcasts** |

### Novel Features & Architectural Highlights:
1. **The Lowest-Cap Ceiling (`min(individual caps)`):** Most group trips overspend because the highest-budget member dominates. Plan B programmatically locks the group ceiling to the lowest member cap, ensuring travel remains accessible to everyone in the group.
2. **Hidden Destination Privacy Shield:** Solves peer friction when planning surprises or sensitive destinations. A member can propose a destination without exposing it on the shared map; Gemini factors it in secretly while being strictly forbidden from naming it.
3. **The Constrained Gemini Guardrail:** The AI is not an open-ended writer; it is an itinerary optimizer restricted to an allow-list of member place IDs. If zero places exist, it immediately halts. It is architecturally prevented from inventing fake restaurants or prices.
4. **Single-Slot Replanning:** When travel disruptions happen, travelers do not need to rewrite the entire trip. Plan B isolates the single broken hour, evaluates candidate replacements from the existing place pool, and patches only that slot—keeping booked flights and hotel reservations permanently locked.
5. **In-Room Conversational AI Concierge (`@PlanB`) & Consensus Summarizer:** In addition to the dedicated Agent workbench (Screen 14), the AI joins the group chat directly. Members can mention `@PlanB` to query shared parameters, settle debts, or synthesize unstructured debate into a structured consensus summary (resolving conflicting pacing or dining desires into one-tap schedule diffs), liberating the trip host from answering repetitive logistics questions or manually reading 100+ chat messages.

---

## 5. Technical Architecture & Feasibility

### Tech Stack

| Layer | Technology Chosen | Why We Chose It | Constraints & How We Address Them |
| :--- | :--- | :--- | :--- |
| **Frontend Framework** | **Nuxt 3 (Vue 3, TypeScript)** | Full-stack Vue ecosystem, native `<ClientOnly>` wrappers for client-side Leaflet rendering, auto-imports, and first-class PWA support via `@vite-pwa/nuxt`. | SSR hydration mismatch with browser-only Leaflet maps. Resolved cleanly using Nuxt's `<ClientOnly>` component boundary. |
| **Backend / Server Engine** | **Nitro Engine (Nuxt 3 Server Handlers)** | Zero-config, ultra-fast serverless route handlers deployed natively to Vercel with minimal cold-start times. | Stateless execution prevents persistent background timers. Handled by reactive client webhooks and Supabase Realtime subscriptions. |
| **Database & ORM** | **Supabase (PostgreSQL) + Drizzle ORM** | Drizzle offers pure TypeScript schema definitions with zero Rust/engine binaries, ensuring instant serverless cold starts. Supabase provides managed Postgres, Row Level Security (RLS), and Realtime websockets. | Connection pooling limits in serverless. Addressed by utilizing Supabase's transaction connection pooler on port 6543. |
| **Realtime Services** | **Supabase Realtime** | Native PostgreSQL change-data-capture websockets for instant chat messaging, shared map pin updates, and live replan broadcast. | Free-tier concurrent websocket connections. Sufficient for hackathon multi-user demo rooms; channels are scoped per trip room ID. |
| **AI Engine** | **Google Gemini (Trip-Scoped)** | High context efficiency, rapid JSON structured outputs, and excellent adherence to strict system instructions. | Potential LLM hallucination. Addressed by enforcing a server-side JSON schema validator that rejects any response containing place IDs outside the trip's allowed pool. |
| **Mapping & Geocoding** | **Leaflet + OpenStreetMap + Photon** | Open-source mapping stack. Leaflet provides smooth tile rendering; Photon (by Komoot on OSM data) provides fuzzy autocomplete without API keys or commercial rate limits. | Photon fair use and coverage edge cases. Mitigated by allowing direct map tapping to drop precise latitude/longitude coordinates anywhere. |
| **Hosting & Deployment** | **Vercel + Supabase Cloud** | Global edge network with zero server maintenance, automated CI/CD from GitHub, and production HTTPS out of the box. | Serverless function timeout limits (10s on hobby). Gemini and Drizzle queries execute within 1.5–2.5s, well within thresholds. |

---

### System Architecture Diagram

```mermaid
flowchart TD
  subgraph CLIENT["Client Layer: Mobile PWA (390px Viewport)"]
    UI["Nuxt 3 PWA UI<br/>(Vue 3 · Pinia · Tailwind)"]
    MapClient["Leaflet OSM Engine<br/>(<ClientOnly> Tile Canvas)"]
    RTClient["Supabase Realtime Client<br/>(Chat & Disruption Webhooks)"]
  end

  subgraph SERVER["Serverless Backend Layer (Nitro Server Engine on Vercel)"]
    AuthRoute["/api/auth<br/>Session Validation"]
    AlignRoute["/api/trips/align<br/>Date Overlap & Min-Cap Math"]
    GeoRoute["Photon Geocoding Proxy<br/>(Fuzzy Autocomplete API)"]
    GeminiRoute["/api/gemini/schedule<br/>Allow-List Grounded Planner"]
    ReplanRoute["/api/gemini/replan<br/>Single-Slot Patch Engine"]
    DebtRoute["/api/money/settle<br/>Graph Minimization Math"]
  end

  subgraph STORAGE["Data & Security Layer (Supabase Managed Cloud)"]
    DB[(PostgreSQL Database)]
    RLS["Row Level Security (RLS)<br/>Private Destination Isolation"]
    RTServer["Supabase Realtime Engine<br/>(Postgres CDC Websockets)"]
  end

  subgraph EXTERNAL["External AI & Geocoding Services"]
    GeminiAPI["Google Gemini API<br/>(Constrained JSON Mode)"]
    PhotonAPI["Photon OSM Geocoder<br/>(OpenStreetMap Elasticsearch)"]
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

  GeminiRoute -.->|Strict ID Validation| DB
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

To ensure 100% technical feasibility, the scope for the build phase is strictly bounded:

#### In-Scope (What We Plan to Build):
- [x] Full authentication flow with personal travel defaults (pace, dietary restrictions, friend connections).
- [x] Solo and group trip room lifecycle with instant link and 6-digit code sharing (`688422`).
- [x] 4-step preference submission form with the "Hidden Destination" privacy toggle.
- [x] System alignment engine computing date intersections, deal-breaker clashes, and group budget ceiling (`min(individual caps)`).
- [x] Interactive Leaflet OSM map with Photon fuzzy search autocomplete and direct tap-to-pin coordinate saving.
- [x] Server-side Gemini itinerary sequencing constrained to existing place IDs, with a hard halt guardrail if zero places exist.
- [x] Plan Mode (preparation) and Trip Mode (live next-stop countdown) state machine.
- [x] Single-slot replan modal replacing only the disrupted hour while preserving locked flights and hotel stays.
- [x] Itinerary-tied expense logging with automated who-owes-whom debt graph calculation (Balance Equalizer).
- [x] Real-time in-room team group chat powered by Supabase Realtime with place card sharing and live disruption broadcast.
- [x] Mobile-first PWA interface with persistent 6-tab navigation (`Trips · Map · Plan · Money · You · Chat`).

#### Out-of-Scope (Explicit Anti-Features):
- ❌ **Commercial Flight & Hotel Booking APIs:** No live ticket checkouts or dynamic inventory scrapers. Bookings are represented as user-entered, locked anchor items.
- ❌ **Nearby Auto-Scrapers:** The app never pulls unverified external restaurant lists; it schedules only venues intentionally added by travelers.
- ❌ **Dynamic Currency Fare Speculation:** Expense splitting is based on actual logged receipts, not speculative live foreign exchange scrapers.\n