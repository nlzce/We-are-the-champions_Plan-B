# Plan B

**When plan A fails, Plan B saves the trip.**

Group trip workspace + a constrained Gemini agent. One shared record for people, money, and days. If a stop falls through, only that slot is rewritten.

PWA · Nuxt 3 · Vue 3 · Drizzle ORM · pnpm · Node · Supabase · Google Gemini
No flight, hotel, or geocoding APIs. No seed users, trips, or attractions.

| | |
| --- | --- |
| Track | Lifestyle · Planning an Escape |
| Team | **We Are The Champions** |
| Design | _add Figma design link_ · _add Prototype play link_ |
| Ideation | [docs/ideation.md](docs/ideation.md) · [docs/diagrams.html](docs/diagrams.html) |
| Demo video | _YouTube unlisted — title: `We Are The Champions — Plan B — Planning an Escape`_ |
| Slides | _add link_ |
| Live | _add deploy URL_ |

---

## Overview

Trip planning is usually five apps and a group chat: tickets in one place, money in another, the day list in Notes, preferences in WhatsApp. When a plan breaks, none of those tools rewrite just the broken hour.

Plan B is one unified trip room that guides travelers **from start to finish** — whether traveling **solo or in a group**:

- **Account & Profiles:** Login / Register. Profile covers default travel pace, dietary restrictions, and friends entry. No preloaded mock users.
- **Trip Room:** Solo trip or invite a group. Invite via friends list or copy share link (link does not require prior friendship). Trip list can be empty initially.
- **Preference Form:** One submission per member covering destination wishlist, available dates, budget ceiling, pace, interests, deal-breakers (雷区), and must-visit spots.
- **Hidden Destination:** A destination can be marked hidden from other members. It does not drop a pin on the shared map. Only this trip’s Gemini agent may access it, and the agent is strictly forbidden from naming it in outputs.
- **Alignment Engine:** System automatically computes date overlap windows, the group budget ceiling (strictly the lowest individual cap), and common constraints. The group confirms destination **before** generating a day-by-day itinerary.
- **Grounded Dual-Mode Map:** Displays only public, member-added points. Members can search places with **Photon fuzzy autocomplete** (open-source OpenStreetMap geocoder by Komoot) to fly and auto-pin, or tap the map directly for custom spots. Independent open-source architecture with no commercial flight/hotel checkout lock-in. Empty inputs produce an empty map.
- **Constrained Gemini Agent:** Exactly one agent per trip, reading only that trip's data. It can only arrange days using existing place IDs. If zero places exist, it halts; it never hallucinates venue names or prices.
- **Two Operating Modes:**
  - **Plan Mode:** Align member preferences, confirm destination, and schedule itinerary.
  - **Trip Mode:** Live countdown to the next stop, with one-tap "delay" or "cannot go". Mid-trip replanning rewrites only that single disrupted slot; locked flights and hotel stays remain untouched.
- **Money & Splits:** Expenses are logged directly against itinerary items, calculating an exact "who owes whom" debt ledger. No real-time ticket or fare APIs.
- **Real-time Team Group Chat:** In-room chat powered by Supabase Realtime where members discuss plans, share place cards, and receive automatic trip notifications (delays, single-slot replans, expense logs).
- **PWA Interface:** Six bottom navigation tabs: **Trips · Map · Plan · Money · You · Chat** (Chat positioned to the right of You).

### Alignment with the Hackathon Problem Statement

| Problem Statement Requirement | How Plan B Solves It |
| --- | --- |
| **End-to-End Planning** (*"from start to finish"*) | One room replaces five apps: covers profile, preferences, alignment, itinerary, live trip execution, and split settlements. |
| **Syncing Group Preferences** (*"preferences to line up"*) | Structured forms capture dates, caps, pace, and deal-breakers; hidden destinations allow surprise wishlists without peer friction. |
| **Budgeting & Cost Splitting** (*"budgeting... split costs"*) | Upfront group budget cap (`min(individual caps)`) prevents overspend; itemized spend ledger computes who owes whom. |
| **Grounded Itinerary Building** (*"building an itinerary"*) | Gemini sequences real member-added places by pace and budget; hard guardrail halts if zero places exist (no fake shops). |
| **Mid-Trip Last-Minute Adjustments** (*"adjusting plans on the fly"*) | Dedicated **Trip Mode**: one-click delay / cancellation triggers single-slot replanning while preserving booked stays & flights. |
| **Solo vs. Group** (*"whether solo or with a group"*) | Frictionless for solo travelers; group collaboration via share link without requiring mutual friending upfront. |

---

## Problem

Most travel apps own one slice. Groups cannot see how much they are actually carrying (dates, money, places) until it is too late. Mid-trip changes have no owner inside the same product.

**Main gap:** the group has no single trusted trip record.

---

## Who it is for

University students and small friend groups on short, phone-first trips — one person or several. Not “everyone who travels”.

Persona: four people, four caps, one private wishlist, currently using WhatsApp + Notes + a spreadsheet.

---

## What we built

1. **App name:** Plan B. Slogan: *"When plan A fails, Plan B saves the trip."* Icon: glass compass on navy.
2. **PWA UI:** 390px phone-first layout with 6 bottom tabs: **Trips · Map · Plan · Money · You · Chat** (Chat positioned to the right of You).
3. **No seed data:** No mock users, sample trips, or seeded attractions.
4. **Rooms & Joining:** Solo trip or group trip; invite via friends list or copy link (no prior mutual friending required).
5. **Preference Form:** Each member fills destination wish, dates, budget ceiling, pace, interests, deal-breakers, must-visit.
6. **Hidden Destination Privacy:** Destination can be hidden from peers; omitted from map; Gemini reads it privately and cannot reveal its name.
7. **Alignment Engine:** Calculates date overlap, group budget ceiling (= lowest individual cap), and shared constraints.
8. **Destination Confirmation:** Group confirms destination before day-by-day scheduling starts.
9. **Grounded Dual-Mode Map:** Search places with open-source Photon fuzzy autocomplete or tap map to drop custom coordinates. Self-contained architecture without external commercial geocoding dependencies. Zero places = empty map.
10. **Trip-Scoped Gemini:** One agent per trip room, reads only this trip's rows.
11. **Hallucination Guardrail:** Agent schedules only existing place IDs. If no places exist, it halts. Never invents shops or prices.
12. **Two Modes:** Plan Mode (align & schedule) and Trip Mode (next stop & execution).
13. **Single-Slot Replan:** Marking a delay or cancellation rewrites only that disrupted block. Locked flights and stays remain untouched.
14. **Expense Splitting:** Spend logged on itinerary items; computes who owes whom. No dynamic fare lookups.
15. **Real-time Team Group Chat:** In-room chat powered by Supabase Realtime; members discuss plans, share place cards, and receive automatic trip notifications (delays, single-slot replans, expense logs).
16. **Explicit Anti-Features:** No fake seed data, no booking checkouts, no auto nearby venue scraper, no live ticket prices.

### Screens (14)

| # | Screen | What it is for |
| --- | --- | --- |
| 00 | Splash | Brand icon and slogan |
| 01 | Login / Register | Sign in or sign up (no preloaded users) |
| 02 | Trips | Room list; allowed to be empty initially |
| 03 | Trip Home | Alignment hub; switch between Plan Mode and Trip Mode |
| 04 | Preference Form | Destination wishlist, dates, budget cap, pace, interests, deal-breakers, must-visit (hidden toggle) |
| 05 | Map | Public pins; search via Photon fuzzy autocomplete or tap map to drop coordinates (open-source OSM) |
| 06 | Itinerary | Day-by-day itinerary arranged strictly from member-added place IDs |
| 07 | Trip Mode | Next stop countdown, quick expense logger, mark delay / cannot-go |
| 08 | Money | Group budget ceiling and who-owes-whom debt split ledger |
| 09 | Agent | Trip-scoped Gemini chat (restricted to existing places, cannot invent venues) |
| 10 | Profile | Default travel pace, dietary restrictions, and friends entry |
| 11 | Invite | Copy join link or invite directly from friends list |
| 12 | Replan | Replace only the single disrupted slot (locked stays remain unchanged) |
| 13 | Group Chat | Real-time member chat with itinerary place sharing and live disruption alerts |

---

## How it is different

| | Maps lists | Splitwise + chat | **Plan B** |
| --- | --- | --- | --- |
| Itinerary | Personal lists | None | Shared days from real member places |
| Group prefs | Weak | Chat | Structured forms + hidden destination toggle |
| Budget | Separate | After spend | Hard cap first (lowest cap), then splits |
| Replan | Manual | Manual | Patch only the single broken slot |
| Live fares | Yes | No | No, by design |

The twist is not “AI travel”. It is Gemini that is **not allowed to invent the trip**.

---

## Ideation & System Models

All diagrams are written in Mermaid in [docs/ideation.md](docs/ideation.md). You can also open [docs/diagrams.html](docs/diagrams.html) in any browser to view and screenshot them for presentation slides.

| Asset | What it shows |
| --- | --- |
| **Problem Tree** | Three root organization failures → no trusted trip record → overspend, misalignment, fake shops, collapse after delay |
| **Use Case Diagram** | UML boundary, actors (Member, Owner, Gemini), and core use cases from alignment to single-slot replanning |
| **Activity Diagram** | Swimlane workflow across Traveler, Plan B System, and Gemini (handling hidden pins, zero-place stop, and slot-level replan) |
| **User Flow** | Step-by-step navigation flow and mode transitions across all 13 screens |
| **Idea Evolution** | V1 (APIs) → V2 (No booking, Nominatim) → V3 (Photon search + Tap-to-pin + Gemini allow-list). Dropped untrue data |
| **Alternatives** | Booking super-app vs chat bot vs Plan B workspace. Workspace chosen because it solves the actual coordination gap |
| **Mindmap** | Clean Mermaid mindmap: Users & Rooms, Product Core, Privacy, Grounding, Screens, Functions, Stack, Anti-features |

---

## Architecture

```
PWA (Nuxt 3) → Nitro Server Routes / API Handlers → Drizzle ORM → Supabase (Postgres, RLS)
                                                ↘ Gemini (generate + replan only)
                                                   output validated against place IDs
                                                   then written as itinerary rows
```

- **Map tiles:** Leaflet + OSM for display (`<ClientOnly>`). Search powered by Photon open-source geocoder.
- **Hidden places:** Supabase Row Level Security (RLS) ensures other members' clients cannot read hidden destinations. The agent route reads them server-side and must never echo the name.
- **ORM & Type Safety:** Drizzle ORM provides lightweight, zero-binary TypeScript schema definitions and query building with zero cold-start overhead on Vercel.
- **No RAG / vector DB:** Grounding is strictly the trip room's own database rows.
- **Security:** Gemini API key stays on the server.

---

## Tech stack

| Layer | Choice |
| --- | --- |
| Frontend | Nuxt 3, Vue 3, TypeScript, PWA (`@vite-pwa/nuxt`) |
| Package manager | pnpm |
| Runtime | Node.js |
| Backend & Server Routes | Nitro Engine (Nuxt 3 Server Routes / API Handlers) |
| ORM | Drizzle ORM (TypeScript-native, zero-binary, serverless-ready) |
| Data / Auth | Supabase Postgres, Auth, Realtime, RLS (`@nuxtjs/supabase`) |
| AI | Google Gemini, trip-scoped, allow-list of place IDs |
| Map & Geocoding | Leaflet + OpenStreetMap tiles + Photon open-source fuzzy search (no external API keys, `<ClientOnly>`) |
| Hosting | Vercel + Supabase |

---

## Scope

**In:** Trip room, member preference forms, hidden destination toggle, tap map, alignment engine, Gemini generate/replan, Plan/Trip modes, expense splits, PWA.

**Out (Explicit Anti-Features):** Ticket checkout, live inventory, geocoding service, seed demo cities, auto nearby venue scraper.

Infrastructure: Supabase (Auth, Postgres, Realtime), Vercel serverless, OSM tiles, Gemini called strictly on generate and replan.

---

## Setup

```bash
pnpm install
cp .env.example .env
pnpm dev
```

`.env.example` (do not commit real keys):

```
SUPABASE_URL=
SUPABASE_KEY=
DATABASE_URL=
GEMINI_API_KEY=
```

Add to Home Screen from a mobile browser to install the PWA.

### Demo path (no fake data)

1. Register two accounts (no seeded accounts).
2. Owner creates an empty trip and copies the invite link.
3. Both members submit the form. One person hides a destination.
4. Map shows only public pins. Tap map to add a public place.
5. Group alignment confirms destination. Gemini arranges **those** places into days.
6. Switch to Trip mode. Mark cannot-go on a slot. Replan rewrites that slot only (locked stays untouched).
7. Log an expense on an item. See who owes whom.
8. Open Team Group Chat (Chat tab) to see member messages, shared place cards, and live disruption alerts.

---

## Team

**We Are The Champions**

- Track: Lifestyle · Planning an Escape
- Submission: Plan B

Video file / YouTube title must use the same team name: `We Are The Champions — Plan B — Planning an Escape`.

---

## License

Built for the hackathon. All product logic in this repo was written during the event.
