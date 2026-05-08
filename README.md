# AURORA BRITANNIA
## Systemic Open World Engine  //  GTA-Style Prototype

```
╔══════════════════════════════════════════════════════════════╗
║  AURORA BRITANNIA  //  SYSTEMIC OPEN WORLD ENGINE            ║
║  A playable Python / Pygame open-world prototype             ║
╚══════════════════════════════════════════════════════════════╝
```

---

## Quick Start

```bash
pip install pygame numpy
python main.py
```

---

## Controls

| Key           | Action                              |
|---------------|-------------------------------------|
| `W/A/S/D`     | Move on foot / Drive vehicle        |
| `↑/←/↓/→`    | Alternative move / drive            |
| `E`           | Enter / Exit vehicle                |
| `TAB`         | Toggle faction influence overlay    |
| `M`           | Force-generate a new mission        |
| `ESC`         | Quit                                |

---

## World

The map is **4800 × 3600** world-pixels, procedurally generated with
four distinct zones:

| Zone                 | Character                                      |
|----------------------|------------------------------------------------|
| **City Centre**      | Dense road grid, tall buildings, Crown Syndicate |
| **Industrial Corridor** | Factories, sparse roads, Iron Collective     |
| **Transport Hub**    | Rail lines, depots, Free Operators            |
| **Outskirts**        | Parks, low density, Municipal Order            |

---

## Gameplay Systems

### Vehicles
Three vehicle types spawn parked across the map:
- **CAR** — balanced everyday vehicle
- **SPORTS** — fast, harder to control, fragile
- **VAN** — slow, heavy, high durability

Walk up to a parked vehicle (yellow prompt appears) and press **E** to
enter.  Drive with WASD.  Exit with **E** again.

### Missions (Dynamic)
Missions auto-generate every 30 seconds (or press **M**).

| Type          | Objective                                             |
|---------------|-------------------------------------------------------|
| DELIVERY      | Drive to the marked location before the timer expires |
| REACH TARGET  | Reach the goal as fast as possible                    |
| RETRIEVE      | Collect an asset, then deliver it to a drop point     |
| EVADE         | Reach the safe zone while keeping heat below 30       |
| ESCORT        | Protect the operative to the extraction point         |

Completing missions awards **£ cash**, shifts **faction influence**,
and reduces your heat level.

### Heat System
Actions accumulate **heat** (0–100):

| Trigger              | Heat gain           |
|----------------------|---------------------|
| Speeding             | +0.8/s while active |
| Entering restricted zone | +8.0 one-time  |
| Mission action       | +6–12               |
| Mission failure      | +8.0                |
| Being caught         | Clears heat, −£fine |

Heat levels:
`CLEAR → NOTICED → WANTED → PURSUED → HUNTED → MOST WANTED`

Above **30 heat**, NPC pursuers spawn and converge on your position.
Stay out of sight and heat decays naturally.

### Factions
Four factions contest territory across the four zones:

| Faction            | Colour  | Home Zone              |
|--------------------|---------|------------------------|
| Crown Syndicate    | Gold    | City Centre            |
| Iron Collective    | Red     | Industrial Corridor    |
| Free Operators     | Cyan    | Transport Hub          |
| Municipal Order    | Green   | Outskirts              |

Completing missions shifts territory control and your **reputation**
with the relevant faction.  Press **TAB** to see the influence overlay.

### Economy
Each region has a **wealth value** that fluctuates based on:
- Crime level in the zone
- Mission activity
- Natural recovery over time

Wealthier regions offer better mission rewards.

---

## Project Structure

```
aurora_britannia/
├── core/
│   ├── game_loop.py       Main loop (events, dt, render cycle)
│   └── state.py           Central GameState — owns all subsystems
│
├── world/
│   ├── map.py             Tile generation, rendering, collision
│   └── regions.py         Zone definitions, faction/crime state
│
├── player/
│   └── player.py          Player entity (on-foot + vehicle states)
│
├── vehicles/
│   └── vehicle.py         Arcade-physics vehicles + AI steering
│
├── missions/
│   ├── mission.py         Mission data structure
│   └── generator.py       Dynamic mission generation + objective logic
│
├── factions/
│   └── factions.py        Faction reputation + territory simulation
│
├── ai/
│   └── npc.py             Pedestrian AI (wander/flee) + pursuit NPCs
│
├── systems/
│   ├── crime.py           Heat level, wanted response, catch penalty
│   └── economy.py         Regional wealth, price simulation
│
├── ui/
│   ├── hud.py             All HUD elements (heat, speed, missions, cash)
│   └── minimap.py         Pre-baked minimap with live entity markers
│
├── utils/
│   └── config.py          All constants, colours, tuning parameters
│
└── main.py                Entry point
```

---

## Expanding the Engine

The modular architecture makes it easy to add:

- **New vehicle types** — add an entry to `VEHICLE_TYPES` in `config.py`
- **New mission types** — add to `MISSION_TYPES`, weights in `generator.py`
- **New regions / zones** — extend `build_regions()` in `regions.py`
- **Weapons system** — new module in `systems/`, hook into `crime.py`
- **Dialogue / story** — new module in `missions/`, triggered from `generator.py`
- **Persistence** — wrap `GameState` with SQLite snapshots
- **Multiplayer** — expose `GameState.update()` over a WebSocket relay

---

## Requirements

```
Python  3.11+
pygame  2.x
numpy   1.24+
```

No external assets, paid APIs or compiled extensions required.

---

*AURORA BRITANNIA is a prototype simulation engine — not a finished game.
Systems drive gameplay, not scripts.*
