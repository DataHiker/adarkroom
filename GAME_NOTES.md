# A Dark Room — Complete Game Notes

## Project Overview

**Repo:** [doublespeakgames/adarkroom](https://github.com/doublespeakgames/adarkroom)  
**Stack:** Vanilla JavaScript (no framework), jQuery, HTML5/CSS3  
**Play:** Open `index.html` directly in a browser — no server required  
**Version:** 1.3  
**Save system:** localStorage (auto-saves)

### Code Structure

| File | Purpose |
|---|---|
| `script/engine.js` | Core loop, perks, event dispatcher, save/load |
| `script/room.js` | Phase 1 — the dark room, fire, builder, crafting |
| `script/outside.js` | Phase 2 — village, workers, population, traps |
| `script/path.js` | Phase 3 — world map outfitting, embark |
| `script/world.js` | World generation, movement, combat, weapons |
| `script/ship.js` | Phase 4 — the crashed ship, hull/engine repair |
| `script/space.js` | Phase 5 — the escape sequence (asteroid dodging) |
| `script/events/room.js` | Random events in the room (nomad, beggar, master, etc.) |
| `script/events/setpieces.js` | Landmark dungeons (cave, town, city, mines, ship, etc.) |
| `script/events/encounters.js` | Random wilderness combat by distance/terrain |
| `script/events/global.js` | Events available across all phases |
| `script/prestige.js` | New Game+ — carries stores into next run |
| `script/scoring.js` | Score calculation |

---

## Story Synopsis

The game reveals its narrative through environmental clues, NPC dialogue, and item descriptions. There is no cutscene or explicit exposition — you piece it together.

**You are a Wanderer** — a member of an ancient, spacefaring alien species. Your fleet travels between worlds, drills massive boreholes to extract resources, burns and depletes the planet, then moves on. You have been stranded on one such ruined world.

### The Arc

**Phase 1 — The Dark Room**  
You wake in total darkness, alone in a room with a dying fire. A stranger arrives. Over time she becomes a "builder" who speaks less and less. The fire is all that matters. The cold is absolute. This represents your disorientation after being stranded — possibly a crash, possibly deliberately abandoned.

**Phase 2 — The Village**  
As the fire grows, other wanderers find their way to you. You build huts. A village forms. The world outside is a wasteland — scraps of fur, bleached bones, ruined towns. You send workers to hunt, mine, and craft. The buildings you construct (tannery, smokehouse, steelworks, armoury) reveal the scale of what used to exist here.

**Phase 3 — The Wasteland**  
You walk out into the world. What you find:
- **Scorched towns** with looted schools, clinics stripped clean, armed thugs and scavengers
- **Ruined cities** with snipers on rooftops, soldiers still patrolling, squatter colonies living in hospitals
- **Battlefields** littered with both conventional rifles and laser weapons and alien alloy — two sides fought here, one clearly not human
- **Boreholes**: *"a huge hole is cut deep into the earth, evidence of the past harvest. they took what they came for, and left."* — this was done by your fleet
- **The Swamp**: an old wanderer sits in penance. He tells you: *"he speaks of once leading the great fleets to fresh worlds. unfathomable destruction to fuel wanderer hungers."* He chose to stay behind and live with what he did.

**Phase 4 — The Ship**  
Deep in the wasteland you find a crashed wanderer vessel. The game notes: *"lucky that the natives can't work the mechanisms."* You can. You use alien alloy — scraped from the ruins of your own fleet's past work — to reinforce the hull and upgrade the engines. You are preparing to escape.

**Phase 5 — Space**  
You lift off through an asteroid field (a debris cloud left by the fleet) in a real-time dodge minigame. The altitude layers are named: Troposphere → Stratosphere → Mesosphere → Thermosphere → Exosphere → Space. Get through without losing all hull integrity.

**The Ending**  
You escape. The wanderer fleet is waiting. The cycle continues — somewhere, another world is next.

**New Game+ (Prestige)**  
On your next run, you'll find a "destroyed village" — your previous character's stores, partially preserved in the rubble. *"all the work of a previous generation is here, ripe for the picking."* You are the next wanderer who crashed here. Or you are the same one, caught in a loop. The game doesn't say.

---

## Key Decisions & Best Choices

### The Master — Evasion / Precision / Force / Nothing

This is the most important permanent choice in the game.

**Trigger:** An old wanderer arrives and asks for lodgings.  
**Cost:** 100 cured meat + 100 fur + 1 torch  
**When available:** After the world map opens (features.location.world)

Each perk can only be taken once. The event can repeat, so you'll eventually be offered all three (across visits), but you choose one per visit.

#### The Perks

| Choice | Perk | Effect |
|---|---|---|
| **Force** | `barbarian` | Melee weapons deal **more damage** |
| **Precision** | `precise` | **Land blows more often** (higher hit chance) |
| **Evasion** | `evasive` | **Dodge attacks more effectively** (lower chance to be hit) |
| **Nothing** | — | You walk away with nothing |

#### Analysis & Recommended Order

**Take Force first.**

Melee weapons scale extremely well with barbarian. By the time you meet The Master you likely have an iron sword (4 dmg) or steel sword (6 dmg) or bayonet (8 dmg). Barbarian multiplies that. Faster kills = fewer total hits taken = less food/medicine consumed per expedition. This has the highest offensive ROI.

**Take Precision second.**

The base hit chance is 0.8 (80%). Missing 20% of attacks is a significant tax on a run. Precision closes that gap. More consistent than evasion because offense always ends fights sooner, and shorter fights mean less incoming damage regardless.

**Take Evasion third.**

Evasion is genuinely useful, especially against late enemies like snipers (15 dmg/hit, 0.8 hit chance) and commandos (3 dmg but 0.9 hit chance, high health). But since you can heal with cured meat (+8hp) and medicine (+20hp), surviving a longer fight is easier than winning a shorter one offensively. Evasion is a defensive hedge, not a force multiplier.

**Take Nothing only if:** you already have all three perks and are burning the cost to trigger the event anyway — unlikely. Otherwise never take nothing; the cost is cheap enough that the perk is always worth it.

> **TL;DR: Force → Precision → Evasion. Never nothing.**

---

### Other Key Room Events

#### The Nomad (merchant)
Shows up when you have fur. Sells:
- **Scales** — 100 fur → 1 scale (poor value early)
- **Teeth** — 200 fur → 1 tooth (also poor early)
- **Bait** — 5 fur → 1 bait (**always buy** — bait improves trap yield significantly)
- **Compass** — 300 fur + 15 scales + 5 teeth → 1 compass (**essential** — required to navigate the world map; buy immediately when available)

> **Priority: Bait > Compass > skip the rest until you're flush.**

#### The Mysterious Wanderer (gambling)
Arrives with an empty cart and offers to return with more.
- Give 100 wood: **50% chance** to return with 300 (3x) — positive expected value
- Give 500 wood: **30% chance** to return with 1500 (3x) — positive expected value but higher variance
- Turn away: safe

> **Take the 100 wood gamble** when you have surplus. Skip 500 unless you're rich. Same event exists for fur.

#### The Shady Builder
Offers to build a hut for 300 wood (normal cost is 100 + n×50, so this is good value for huts 5+).  
- **60% chance he steals your wood**, 40% chance he actually builds.

> **Skip unless you have wood to spare and don't mind losing it.**

#### The Scout (map/perk merchant)
- **Buy map** — 200 fur + 10 scales → reveals world tiles. Buy early and often.
- **Learn scouting** — 1000 fur + 50 scales + 20 teeth → `scout` perk (see farther on the map)

> **Buy the map every visit.** The scout perk is good but expensive; skip if resources are tight.

#### The Sick Man
Costs 1 medicine. Outcomes:
- 10% chance: **alien alloy** (the rarest resource — extremely valuable)
- 30% chance: **3 energy cells**
- 50% chance: **5 scales** (weak reward)
- 10% chance: nothing

> **Always help him.** The 10% alien alloy chance alone makes the medicine worth it. Alien alloy is needed to repair the ship.

---

### World Map Landmarks

#### Mines (must-clear to unlock workers)
Each mine has a boss fight. Clear them ASAP — they unlock sulphur/iron/coal miners which automate resource generation for the endgame.

| Mine | Boss | Difficulty | Reward |
|---|---|---|---|
| Coal Mine | 2× man + chief | Easy | Coal miners |
| Iron Mine | Beastly matriarch | Medium | Iron miners |
| Sulphur Mine | 2× soldier + veteran | Hard | Sulphur miners |

> Clear iron mine first (easy boss, iron feeds steelworks). Coal second (enables steelworks production). Sulphur last (soldiers hit hard — bring a steel sword or better).

#### The Swamp
Costs 1 charm (rare trap drop). Reward: **`gastronome` perk** — restores more health when eating.

> **Always visit if you have a charm.** Gastronome extends your expeditions significantly by making cured meat more efficient.

#### Abandoned House
25% chance: medicine cache. 50% chance: loot + free water refill. 25% chance: squatter fight.  
> Always enter. Free water refills save runs.

#### Battlefield
No fight — just loot: rifles, bullets, laser rifles, energy cells, grenades, alien alloy.  
> **High priority landmark.** Visit every one. Alien alloy here is a significant time-saver.

#### Borehole
No fight. Always drops **alien alloy (1-3)**. One of the most reliable alien alloy sources.  
> Always visit.

#### The Crashed Ship
Marks itself on the map and draws a road. The entire endgame builds toward this.

---

### Weapons Progression

| Weapon | Damage | Cooldown | Notes |
|---|---|---|---|
| Fists | 1 | 2s | Fallback only |
| Bone Spear | 2 | 2s | Craftable early; skip if possible |
| Iron Sword | 4 | 2s | First real weapon; looted from dead wanderers |
| Steel Sword | 6 | 2s | **Best melee**; looted in towns/cities |
| Bayonet | 8 | 2s | Best melee, rare drop from veterans |
| Rifle | 5 | 1s | Fast, uses bullets; good mid-game |
| Laser Rifle | 8 | 1s | **Best ranged**; uses energy cells |
| Grenade | 15 | 5s | Massive burst; costs grenades |
| Bolas | Stun | — | No damage, just stuns; niche use |

> **Best loadout:** Laser rifle (primary) + steel sword or bayonet (backup) + bolas (emergency stun). With barbarian perk, the bayonet at 8 base damage becomes devastating.

---

### Armour Progression

| Armour | How to Get | Benefit |
|---|---|---|
| None | — | No protection |
| Leather | Craftable (200 leather + 20 scales) | Small damage reduction |
| Iron | Craftable (200 leather + 100 iron) | Better reduction |
| Steel | Craftable (requires steelworks) | Good reduction |
| Kinetic | Late-game/rare | Best available |

> Craft iron armour before doing sulphur mine or city runs. The soldier encounters hit hard.

---

### Carry Capacity (critical for long expeditions)

| Upgrade | Cost | Bag Space |
|---|---|---|
| Default | — | 10 |
| Rucksack | 200 leather | +10 (total 20) |
| Wagon | 500 wood + 100 iron | +30 (total 40) |
| Convoy | 1000 wood + 200 iron + 100 steel | +60 (total 70) |
| Cargo Drone | Late game | +100 (total 110) |

> Rucksack first, then wagon as soon as possible. Convoy before the final ship run. More carry = more healing supplies = longer survival radius.

---

### Water Management (expeditions live and die by this)

- **No upgrade:** 10 water
- **Waterskin** (50 leather): small increase
- **Cask** (100 leather + 20 iron): medium
- **Water Tank** (100 iron + 50 steel): maximum

You consume 1 water per move. With a water tank, a full load gets you ~30+ tiles from the village — enough to reach every landmark. Without one, you die of thirst before the city.

> Craft water tank before doing any deep-map exploration.

---

---

## Achievements / Progress Milestones

> **Note:** The web version (this repo) has **no built-in achievement system** — the source code explicitly has a comment: *"trophies (in future), achievements (again, not yet)"*. The mobile apps (iOS/Android) have formal achievements tied to Game Center / Google Play, but those are not in this codebase.
>
> What follows are the internally tracked progress events and meaningful milestones worth hitting, in the order you'll encounter them in an optimal run.

### Tracked Progress Events (internal)

These fire `Engine.event('progress', ...)` — they're the game's own milestone markers:

| Milestone | Trigger | When |
|---|---|---|
| `new game` | First load with no save | Start |
| `outside` | Stranger arrives and you go get wood | Early |
| `path` | Compass acquired, world map opens | Mid |
| `dungeon cleared` | Complete any landmark dungeon | Mid |
| `coal mine` | Clear the Coal Mine boss | Mid |
| `iron mine` | Clear the Iron Mine boss | Mid |
| `sulphur mine` | Clear the Sulphur Mine boss | Mid-Late |
| `ship` | Find the Crashed Ship landmark | Late |
| `fabricator` | Take the device from the Executioner battleship | Late |
| `win` | Escape through the asteroid field | Endgame |
| `crash` | Die in the asteroid field | Failure state |

---

### Full Achievement Checklist (Optimal Order)

#### Phase 1 — The Room

- [ ] **Light the fire** — stoke to "roaring" for the first time
- [ ] **The builder arrives** — stranger becomes builder after you bring wood
- [ ] **Build a trap** — first crafted item
- [ ] **Build a hut** — first building; unlocks population growth
- [ ] **The thief event** — occurs after enough huts (~5+). Two outcomes:
  - *Hang him* — supplies returned, no perk
  - **Spare him** — grants `stealthy` perk (better conflict avoidance in the wild) ← **take this**

#### Phase 2 — The Village

- [ ] **Assign first worker** — any job
- [ ] **Build lodge** — unlocks hunters
- [ ] **Build trading post** — nomad arrives more reliably
- [ ] **Build workshop** — unlocks fine crafting
- [ ] **Build steelworks** — enables steel production
- [ ] **Build armoury** — enables bullet production

#### Phase 3 — The World

- [ ] **Acquire compass** (from Nomad) — opens the world map. Required to continue.
- [ ] **Clear Coal Mine** — progress milestone; unlocks coal miners
- [ ] **Clear Iron Mine** — progress milestone; unlocks iron miners
- [ ] **Clear Sulphur Mine** — progress milestone; unlocks sulphur miners (hardest mine — soldiers + veteran)
- [ ] **Find a Battlefield** — no fight, free loot including alien alloy and laser rifles
- [ ] **Find a Borehole** — free alien alloy (1-3), always
- [ ] **Visit the Swamp** — costs 1 charm; grants `gastronome` perk (more health from eating). Charm is a rare trap drop (0.5% chance).
- [ ] **Help the Sick Man** — 10% chance of alien alloy reward; always worth 1 medicine
- [ ] **Meet The Master** (3 visits needed for all perks):
  - [ ] Visit 1 → **Force** (`barbarian` perk)
  - [ ] Visit 2 → **Precision** (`precise` perk)
  - [ ] Visit 3 → **Evasion** (`evasive` perk)
- [ ] **Learn scouting** from the Scout event — `scout` perk (see farther on map)
- [ ] **Find the Crashed Ship** — progress milestone; unlocks the Ship tab

#### Phase 4 — The Battleship (Executioner) — Late Game

This is a multi-visit mega-dungeon at the very edge of the map (radius 28 — maximum distance). It is the hardest content in the game and the only source of certain blueprints.

**First visit — The Ravaged Battleship (intro)**

Fight through three possible paths (chitinous horrors, operatives, or ancient beast), then power-cycle a maintenance panel, defeat an automated turret, and retrieve *a strange device*. This unlocks the **Fabricator** tab back at your ship.

> **Reward:** Fabricator unlocked; fleet beacon quest begins

**Subsequent visits — The Antechamber**

Three wings must be cleared before the Command Deck opens. Wings can be done in any order but all three are required.

| Wing | Final Boss | Blueprint Reward | Other Notes |
|---|---|---|---|
| **Engineering** | Unstable Prototype (150 HP, shields) | `kinetic armour` + `hypo` blueprints | Has a healing machine (costs 1 alien alloy, full heal) |
| **Medical** | Malformed Experiment (200 HP, enrages at 50%) | `stim` + `glowstone` blueprints | Broken medic drones go venomous at 50% HP — watch out |
| **Martial** | Murderous Robot (250 HP, energises at ~70%) | `disruptor` + `plasma rifle` blueprints | Blowing the sealed door (costs 1 grenade) gives a full weapon rack |

- [ ] **Clear Engineering Wing** — kinetic armour + hypo blueprints
- [ ] **Clear Medical Wing** — stim + glowstone blueprints  
- [ ] **Clear Martial Wing** — disruptor + plasma rifle blueprints

**Command Deck — The Immortal Wanderer**

After all three wings are cleared, the Command Deck opens. A squat figure sits motionless at the centre — a wanderer that *"can't die"*, running alternating shields / enrage / meditation specials every 7 seconds. 500 HP.

- [ ] **Defeat the Immortal Wanderer** — drops the **fleet beacon** (worth 500 score, and triggers a special end-of-game variant)

> This is the hardest fight in the game. Bring plasma rifle, full hypos, kinetic armour, and every perk you can get. The disruptor can interrupt its special states.

#### Phase 5 — The Ship & Escape

- [ ] **Reinforce hull** (alien alloy × n) — minimum 1 hull point to be able to lift off
- [ ] **Upgrade engines** — each upgrade increases asteroid dodge speed; more upgrades = easier escape
- [ ] **Lift off and escape** — dodge asteroids through Troposphere → Stratosphere → Mesosphere → Thermosphere → Exosphere → Space. Asteroids spawn faster and in greater numbers at higher altitude.
- [ ] **Win** — escape with hull > 0

#### New Game+ Milestones

- [ ] **Find the destroyed village** on your second run — collects your previous run's stores (weapons and goods, slightly randomised)
- [ ] **Beat your previous score** — scores are cumulative across runs via prestige

---

### Unique / Missable Events

These have no persistent flag — you can miss them in a run if you don't have the right resources:

| Event | Requirement | Reward | Miss condition |
|---|---|---|---|
| Swamp (gastronome) | 1 charm in inventory | `gastronome` perk | Charm is a 0.5% trap drop — very rare |
| The Master (perks) | 100 cured meat + 100 fur + 1 torch | `barbarian`/`precise`/`evasive` | Runs out of unlearned perks after 3 visits |
| Sick Man | 1 medicine | Chance of alien alloy | Need medicine available |
| Thief (stealthy) | Game reaches ~5 huts | `stealthy` perk | Must choose "spare" not "hang" |
| Blow martial door | 1 grenade | Full weapon rack (energy blades, plasma rifle) | Grenade must be in inventory when inside the wing |
| Engineering heal machine | 1 alien alloy | Full HP restore | Must have alien alloy to spend |

---

## Scoring

Score is calculated from stores at the end of a run, with heavy multipliers on weapons and endgame items:

- Alien alloy: ×10 per unit
- Fleet beacon: ×500 (extremely rare)
- Hull reinforcement: ×50 per level
- Weapons (iron sword ×10, steel sword ×30, bayonet ×50, rifle ×100, laser rifle ×150)

High scores come from hoarding weapons, alien alloy, and building the ship up before escaping.

---

## Optimal Game Flow Summary

1. Stoke fire → light room → get builder
2. Build traps → gather wood → build huts → grow village
3. Assign gatherers → hunters → tanners → charcutiers in that order
4. Build: cart → lodge → trading post → tannery → smokehouse → workshop → steelworks → armoury
5. Craft: waterskin → rucksack → wagon → water tank → cask/convoy
6. Open world map (compass from nomad is required)
7. Clear coal mine → iron mine → sulphur mine
8. Explore: houses (water), battlefields, boreholes (alien alloy), swamp (gastronome perk)
9. Get The Master's perks: Force first, Precision second, Evasion third
10. Help sick men (alien alloy chance)
11. Find the crashed ship → gather alien alloy → reinforce hull → upgrade engines
12. Equip best loadout → embark → avoid asteroids → escape
