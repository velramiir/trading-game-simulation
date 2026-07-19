# Trading Game — Economy Simulation

This repo is a **pre-prototype simulation**. Its only job is to explore economy
balance: test how parameter changes affect player progression *before* any game
code is written. It is not the game — it isolates the core trading loop so the
numbers can be tuned in a fast, headless environment.

---

## Game Context (what this is a simulation *of*)

A single-player trading game played in sessions on a procedurally arranged node
map. Buy low, sell high. Profit funds vehicle upgrades and home base growth. No
overarching plot — narrative emerges from trade, quests, and characters.

### World
- **Node map** — discrete locations connected by routes. Layout varies per session; nodes are predefined but can vary in parameters.
- **Two continents** — Continent A (land, accessible from start), Continent B (requires a ship). Inter-continental trade is higher risk, higher reward.
- **Two currencies** — Land Coin (LC) on Continent A, Sea Coin (SC) on Continent B. Exchange at Harborport at a loss (1 SC = 0.8 LC).

### Vehicles
| Vehicle | Notes |
|---|---|
| Wagon | Starting vehicle, land only |
| Ship | 200 LC at Harborport, unlocks Continent B |
| Airship | Post-prototype |

Each vehicle carries its own cargo and money. Capacity limits cargo only — money has no weight.

### Resources
| Resource | Notes |
|---|---|
| Grain | Continent A staple |
| Timber | Continent A staple |
| Iron | Continent A staple |
| Spices | Produced on Continent B |
| Fish | Produced on Continent B — spoils in transit |

### Economy (in the full game)
- Prices vary between nodes — the spread is the opportunity.
- Travel time is a risk — prices may shift before you arrive.
- Inter-continental crossing adds weather/delay risk.

---

## What This Simulation Does

Runs a decision strategy against a fixed set of tunable parameters and records how
the economy plays out over time. One hardcoded strategy to start; alternatives can
be added later to compare behaviour under identical parameters. The strategy is the
thing under test — everything else is scenery held constant.

Time is modelled as **continuous seconds**, not days. Travel is real-time: a
vehicle departs a node and arrives at a calculated later time.

### Simulation Scope

**In:** the core trading loop — travel, buy/sell, price spread between nodes,
spoilage, the LC↔SC exchange, and the ship (200 LC) as a milestone target.

**Out (deliberately isolated away):**
- AI traders
- Economy events (shortages, gluts, demand spikes, storms)
- Quests
- Multiple vehicles / automated routes
- Home base mechanics

> Note: the full game includes economy events, quests, multiple vehicles, and home
> base growth. They are excluded *here* on purpose — this sim tests the loop in
> isolation before those systems are layered in.

---

## Tunable Parameters

All set before a run. These are the levers the sim exists to explore.

| Parameter | Default | Notes |
|---|---|---|
| Starting money | 50 LC | Loaded onto the vehicle at session start |
| Wagon capacity | 4 units | Cargo only |
| Ship cost | 200 LC | Milestone target |
| Exchange rate | 0.8 LC per SC | Applied at Harborport |
| Fish spoilage | scaled per second | Tune to match intended travel time |
| Travel times | per route | Key variable to tune |
| Node prices | per resource per node | Core balance lever |
| Decision delay | 0 | Seconds of player think time; 0 = instant decisions |

---

## What It Measures

Recorded per arrival: time elapsed, money on vehicle, cargo, node visited,
resources bought/sold and at what price, and the route taken.

Derived after a run:
- LC (or equivalent) per second
- Time to reach the ship (200 LC milestone)
- Route frequency — which routes the strategy actually uses
- Capital growth curve over time

A run ends at a fixed time limit or when a milestone is hit (ship bought, or
600 LC equivalent).

---

## Questions the Simulation Should Help Answer
- Is the profit margin interesting but not obvious?
- Does the triangle/route structure reward planning without forcing one dominant route?
- Does the ship feel like an achievable first milestone?
- Does Continent B read as a real upgrade, or just more of the same?
