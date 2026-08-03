# 🌙 Moonlight City — Game Design Document

> A cozy 2D city life & adventure game inspired by Avatar World, The Sims, GotchaLife, and classic dungeon crawlers.

> 📖 **Narrative, NPC backstories, quest arcs and sample dialogue live in [STORY.md](./STORY.md).**

---

## 🎯 Core Vision

Moonlight City is a top-down 2D game where players create a character, live in a vibrant city, build and decorate their home, befriend NPCs through conversation, shop, explore, and descend into a 100-level progressive dungeon system beneath the city streets.

**No farming. This is a city life + adventure game.**

---

## 🧭 Pillars

| Pillar | Description |
|--------|-------------|
| 🏙️ City Life | Explore a living city with shops, homes, districts, and NPCs |
| 🏠 Housing | Buy, build, and decorate your home (Sims-inspired) |
| 🧑 Character | Deep character creation with outfits, accessories, and style |
| 💬 Social | Talk to NPCs with dialogue trees (GotchaLife-inspired) |
| ⚔️ Combat | Action combat with skills, spells, and techniques |
| 🏰 Dungeons | 100-level progressive dungeon with bosses and loot |
| 🔨 Crafting | Combine materials into gear, items, and furniture *(later phase)* |

---

## 🔄 Core Game Loop

> **The one-sentence loop:** *Go down scary, come back rich, make home warmer, get noticed for it.*

Everything in Moonlight City hangs off a single rhythm: **Descend → Return → Spend → Show Off.** The city is where you *are somebody*; the dungeon is where you *earn it*. Neither half works alone, and no system ships unless it plugs into this rhythm.

```
        ┌──────────────────────────────────────────────┐
        │                                              │
        ▼                                              │
   🏠 HOME  ──►  🏙️ CITY  ──►  🚽 DUNGEON  ──►  💰 HAUL ─┘
  (sleep,      (quests,      (3-5 floors,     (gold, loot,
   gear up,     shops,        combat,          materials,
   decorate)    NPCs)         chests, boss)    cosmetics)
        ▲                                          │
        └──────── 👑 SHOW OFF (wear it, place it) ──┘
```

### ⏱️ 1. Moment-to-Moment Loop — the 10-Minute Session

A player should be able to sit down, play for ten minutes, and stand up having **changed something visible**. That is the contract. A session that ends with nothing new in the wardrobe, the house, or the friendship log is a design bug.

**The 10-minute anatomy (target pacing):**

| Time | Beat | Feeling |
|------|------|---------|
| 0:00–1:00 | **Wake up.** Bed → wardrobe → grab potions from the storage chest. Read any note left on the door. | Cosy, in control |
| 1:00–2:30 | **One errand.** Check the bulletin board, talk to *one* NPC, buy potions from Ella. Pick up a "fetch me 5 slime globs" request. | Purposeful |
| 2:30–3:00 | **The descent.** Walk through the Slums, down the sewer grate, ride the Depth Elevator to your deepest checkpoint. | Anticipation |
| 3:00–8:00 | **The dive.** Clear 3–5 floors. Fight, dodge, smash barrels, find the hidden side room, open the chest. One "ooh!" moment guaranteed per floor (a chest, a rare drop, a mini-boss, a lore scrawl). | Tension + delight |
| 8:00–9:00 | **The extraction.** Hit the next checkpoint, warp home. Bank the gold. | Relief, pride |
| 9:00–10:00 | **The spend.** Buy the hat. Place the new lamp. Hand the slime globs to Ella. Watch the friendship bar tick up. | Reward, ownership |

**Core micro-loop inside the dungeon (~30–60 seconds):**
`enter room → assess enemies → attack / dodge / use skill → clear → loot the room → choose the next door`

**Opinionated rules that make this work:**

- **No stamina or energy meter.** The only limiter is your potion supply and your nerve. A child should never be told "you can't play any more today."
- **Checkpoint every 5 floors.** The Depth Elevator unlocks at floors 5, 10, 15, 20, 25. You never re-walk cleared ground.
- **You can leave at any time.** A "Return Stone" (cheap, reusable, 30s cast) teleports you home from anywhere. Sessions end when the *player* wants, not when the dungeon allows.
- **Defeat is soft.** On 0 HP you wake up in Rex's tavern. You **keep every item and all floor progress**, and lose 20% of the gold you were carrying (gold banked in your home chest is always safe). The lesson is "bank your gold," not "you wasted an hour."
- **Every floor pays.** Minimum guaranteed drop per floor: gold + one material. Bad luck is allowed to be boring; it is never allowed to be *nothing*.

### 🌅 2. Daily Loop — One In-Game Day

**A full day = 24 real minutes** (1 in-game hour ≈ 60 real seconds). Long enough to feel like a day, short enough that a child can experience morning-to-night in one sitting.

**The clock only runs in the city.** Time freezes the moment you enter the dungeon. This is a deliberate, load-bearing decision: it means a long dive can never make you miss the festival, the night market, or your friend's birthday. The city is a schedule; the dungeon is a sanctuary from the schedule.

| Segment | Hours | What the city offers |
|---------|-------|----------------------|
| 🌄 **Morning** | 06:00–11:00 | Shops open. Bulletin board refreshes with 3 new requests. NPCs on their commute — best time for quick chats. Furniture deliveries arrive at your door. |
| ☀️ **Afternoon** | 11:00–17:00 | Prime dive window. Guild hall open for class training. Park and docks busy — Lumi is painting, traders at the wharf. |
| 🌆 **Evening** | 17:00–21:00 | The social hour. Tavern fills up. NPC hangout events, cooking at home, invite a friend over to see your house. Highest relationship gains. |
| 🌙 **Night** | 21:00–06:00 | The city changes character. Black Market opens in the Slums. Rose appears at the docks with dungeon lore and maps. Rare night-only NPCs and stargazing spots. Sewer enemies are ~15% tougher but drop *Moonlit* material variants. |

**Sleep is the ritual.** Getting into your own bed = save the game, skip to morning, and gain the **Well-Rested** buff (+10% XP and +1 potion slot for the next dive). Sleeping in a bed you *bought and placed yourself*, in a room you decorated, is the moment the two halves of the game shake hands.

**The intended daily rhythm:** wake → check the board → shop & socialise → one big dive → come home heavy with loot → spend the evening turning that loot into a nicer house and better friendships → sleep. Roughly **35% city / 55% dungeon / 10% home**, and the player never notices the split because the gold pulls them through it.

### 🏆 3. Long-Term Loop — 10+ Hours

Four progression tracks run in parallel, deliberately at different speeds, so something is always about to pay out.

| Track | Pace | The hook | 10-hour milestone |
|-------|------|----------|-------------------|
| ⚔️ **Depth** | The engine | Floors 1→25, then a locked door to a whole new biome | Slime King defeated, Crypts key in hand |
| 🏠 **Home** | Slow burn | Starter Shack → Cottage → Townhouse → Manor; every room is a canvas | Cottage fully furnished, trophy shelf started |
| 💬 **Bonds** | Steady drip | Stranger → Acquaintance → Friend → Best Friend, each tier unlocking a *service* | 2–3 NPCs at Friend, Rex teaching advanced skills |
| 👗 **Collection** | Dopamine | Wardrobe, dyes, boss cosmetics, furniture sets | 30+ cosmetics, one legendary |

**What actually keeps them coming back:**

1. **The 25-floor gate.** Zone 1 is a complete, satisfying 8–12 hour story with a real boss at the end. The Slime King's key is a *promise* — the Crypts are visibly locked behind that door from floor 1, and you walk past it every dive.
2. **Trophies are public.** Boss cosmetics like the **Slime Crown** are not stat items — they are *social objects*. Wear the Crown in the city and NPCs have new dialogue about it. Mount the Slime King's trophy in your house and visiting NPCs comment on it. The reward for beating a boss is being **recognised**.
3. **Milestone collapse.** House tiers, friendship tiers, class skill unlocks and depth milestones are deliberately staggered so a payoff lands roughly every 20–30 minutes of play, from a different track each time.
4. **The city changes back.** Festivals, seasonal decorations, a Mayor's questline that literally rebuilds a district, and NPC storylines that only advance because *you* went down there. The player's fingerprints stay on the world.
5. **Second class, second run.** At Best Friend with the guildmaster, unlock a class swap (keeping your level). Blade Dancer → Mage is a whole new combat feel over the same content.

**Co-play design charter (parent + child):** the loop must support **two seats, one save.** Practical rules: the dungeon supports drop-in local co-op (second player joins as a second class, shared loot, shared XP); the house is jointly owned and both players can place furniture; downed players are *revived by their partner* rather than kicked out, so a stronger parent can carry a struggling child without either of them losing the run. The natural family ritual we are designing for is **"one dive together, then we each decorate a room."**

### 🔗 4. Connective Tissue — How City and Dungeon Feed Each Other

If either half can be skipped, the design has failed. Five hard couplings, in both directions:

**Dungeon → City (the loot must land somewhere):**
- **Gold is the artery.** Dungeon gold is the *only* meaningful income. Furniture, house upgrades, clothes and gifts all cost gold. Every lamp in your house is a slime that died for it.
- **Materials become furniture and gifts.** Sewer materials (slime globs, cracked pipes, rat pelts, glowing fungus) are shop-sold, quest-turned-in, and — in the crafting phase — become *decor*. A Fungus Lantern in your living room is a souvenir.
- **Cosmetics become identity.** Boss drops are worn in the city. The wardrobe is the trophy case.
- **Lore flows upward.** Scrawls and artefacts found below unlock new dialogue with Rose and Rex. The dungeon is where the city's story is buried.

**City → Dungeon (the city must make you stronger):**
- **Friendship unlocks power.** Ella at Friend sells greater potions; Rex at Friend teaches a class technique and marks a secret room on your map; Rose at Friend sells floor maps that reveal chest locations. **Being sociable is a build choice.**
- **Home grants buffs.** Sleeping in your bed = Well-Rested. A cooked meal from your kitchen = a temporary stat buff. A decorated "Trophy Room" grants a small permanent bonus. Decorating is not a side hobby — it is *pre-dive preparation*.
- **Quests aim the dive.** The bulletin board and NPC requests turn an aimless grind into a mission: *"Bring Lumi 5 glowing fungi and she'll paint your living room."* You go down with a shopping list.
- **Shops gate depth.** You cannot survive floor 20 in floor-5 gear. The economy forces you back up into the market.

**The design test for every new feature:** *does it either (a) give the player a reason to go down, or (b) give the loot a place to land?* If neither, cut it.

### 💖 5. The Core Emotional Fantasy

> ## *"Come home a hero."*

Not "be a powerful hero." Not "have a cosy life." **Both — and specifically the moment between them.**

The fantasy Moonlight City sells is the feeling of walking back up the sewer stairs at dusk, muddy and slime-covered, pockets full of gold, and stepping into a warm city where someone says *"whoa — where did you get that crown?"* Then going home to a house you built, putting your trophy on the shelf, and sleeping in your own bed.

It is the fantasy of **being brave out there and being loved in here** — that adventure is meaningful *because* there is a home and a set of people to bring it back to. The danger makes the cosiness feel earned; the cosiness makes the danger feel worth it.

For our target players — a parent and a child at the same screen — this is exactly right. The child gets to be the brave one. The parent gets to be the one who builds the home. And both halves are the same game.

**Anti-fantasies we explicitly reject:**
- ❌ Grim survival, punishing loss, or gear-destroying death
- ❌ Farming, crop timers, or chore simulation
- ❌ Time pressure, missable content, or FOMO-driven daily windows
- ❌ Loneliness — the city must always feel *populated and pleased to see you*

---

## 🌆 The City — Moonlight City

The overworld is a living city with distinct districts:

### Districts
- **Downtown** — shops, the marketplace, the guild hall
- **Residential** — housing plots where the player and NPCs live
- **The Docks** — waterfront area, fishing, sailors, traders
- **The Slums** — darker area near the sewer entrance, shady NPCs
- **The Sewers** *(entrance in The Slums)* — gateway to the dungeon system

### City Features
- Day/night cycle affects which NPCs are out and what shops are open
- Dynamic NPC schedules (NPCs move around the city on routines)
- Seasonal events and city-wide festivals
- Bulletin board with quests and community tasks

---

## 🏠 Housing System *(Sims-inspired)*

### Core Features
- Player purchases a housing plot in the Residential district
- Houses have an **exterior** (visible on city map) and **interior** (separate room view)
- Interior uses a grid-based furniture placement system
- Furniture and decorations can be bought from shops or crafted

### Housing Tiers
| Tier | Name | Description |
|------|------|-------------|
| 1 | Starter Shack | One small room |
| 2 | Cottage | Two rooms, small yard |
| 3 | Townhouse | Three rooms, upstairs |
| 4 | Manor | Full house, garden, basement |

### Interior Systems
- Place/rotate/remove furniture freely
- Wall colours and flooring customisation
- Lighting — candles, lanterns, magic lights
- Storage chests (link to player inventory)
- House level affects NPC visit frequency and social reputation

---

## 🧑 Character Creation & Customisation *(Avatar World-inspired)*

### Creation Options
- **Body** — skin tone, body shape, height
- **Face** — eye shape/colour, nose, mouth, freckles, blush
- **Hair** — style, colour (solid or gradient), length
- **Outfit** — top, bottom, shoes, accessories, hat, bag
- **Expression style** — cheerful, cool, mysterious, etc.

### Ongoing Customisation
- Outfits can be changed any time at home or in a changing room
- New clothing available in shops and as dungeon loot
- Rare cosmetics drop from bosses (e.g. **Slime Crown** wearable cosmetic)
- Character card shown to NPCs and other players (future multiplayer hook)

---

## 💬 NPC Conversation System *(GotchaLife-inspired)*

### Dialogue Features
- Tap/click an NPC to open a conversation bubble UI
- NPCs have **personality types** (friendly, shy, snarky, mysterious)
- Dialogue trees with player choices that affect relationship
- NPCs remember past conversations and reference them later
- Relationship meter per NPC (Stranger → Acquaintance → Friend → Best Friend)

### NPC Schedules
- NPCs move around the city on a daily schedule
- Different dialogue depending on time of day, location, and relationship level
- Some NPCs only appear at night or in specific districts

### NPC Roster (initial cast)

> Full backstories, 3-stage personal quest arcs, and dialogue samples: **[STORY.md](./STORY.md)**

| Name | Personality | Location | Hook |
|------|-------------|----------|------|
| Ella | Cheerful shopkeeper | Downtown market | Sells potions |
| Rex | Gruff ex-adventurer | The Slums tavern | Hints about sewers |
| Lumi | Shy artist | Residential park | Gives decoration items |
| Rose | Mysterious figure | Docks at night | Dungeon lore |
| Mayor Aldric | Pompous | City Hall | City quests |
| Pipper | Excitable kid | Downtown fountain | Bulletin board & crayon maps |
| Auntie Fen | Warm herbalist | Slums clinic | Free healing |
| Will | Snarky rival | Guild Hall | Speedrun challenges |

---

## ⚔️ Combat System

### Overview
- Real-time action combat (not turn-based)
- Player has a **class** chosen at character creation (changeable later at guild)
- Combat happens in dungeon floors and triggered encounter zones

### Stats
- **HP** — health points
- **MP** — mana for spells
- **STR** — physical damage
- **AGI** — speed and dodge chance
- **INT** — spell power
- **DEF** — damage reduction

### Combat Actions
| Action Type | Description |
|-------------|-------------|
| Basic Attack | Fast, low-damage physical strike |
| Skills | Class-specific active abilities (cooldown-based) |
| Spells | Mana-cost magic attacks and buffs |
| Techniques | Charged or combo-based special moves |
| Dodge | Short dash to avoid attacks |

### Classes (Initial)
| Class | Style | Example Skills |
|-------|-------|----------------|
| Blade Dancer | Fast melee | Spin Slash, Shadow Step, Blade Storm |
| Mage | Ranged spells | Fireball, Frost Nova, Arcane Surge |
| Guardian | Tank/defensive | Shield Bash, Iron Wall, Provoke |
| Rogue | Stealth & burst | Backstab, Smoke Bomb, Poison Blade |

### Progression
- Defeat enemies to gain XP and level up
- Level up grants stat points and unlocks new skills
- Equipment drops from enemies and bosses improve stats

---

## 🏰 Dungeon System — 100 Levels Progressive

### Structure
The dungeon is a single continuous system beneath Moonlight City. Progress is saved — you always re-enter at your deepest cleared floor.

```
Floors 1–25    → The Sewers
Floors 26–50   → The Underground Crypts       (future)
Floors 51–75   → The Magma Depths             (future)
Floors 76–100  → The Shadow Realm             (future)
```

---

### 🚽 Zone 1: The Sewers (Floors 1–25)

**Theme:** Dark, damp, dripping tunnels under the city. Slime-covered walls, broken pipes, lantern light.

**Atmosphere:** Eerie but not terrifying — suitable for all ages. Mysterious and a little gross (in a fun way).

#### Sewer Enemies
| Enemy | Floors | Description |
|-------|--------|-------------|
| Slime (small) | 1–10 | Slow, basic, bouncy green blob |
| Sewer Rat | 1–15 | Fast, low HP, attacks in groups |
| Toxic Slime | 10–20 | Inflicts poison status |
| Pipe Spider | 15–25 | Drops from ceilings, webs player |
| Giant Slime | 20–25 | Mini-boss variant, splits into smaller slimes |

#### Sewer Floor Features
- Puddles of slime that slow movement
- Breakable barrels and crates with loot
- Hidden side rooms with chests
- Hazard pipes that spray water/slime

#### Zone 1 Boss — The Slime King 👑

> *"From the deepest filth of the sewers, a consciousness has formed — ancient, patient, and absolutely disgusting."*

**Floor:** 25

**Battle Phases:**
1. **Phase 1 (100–60% HP):** Stomps and throws slime balls. Spawns small slimes.
2. **Phase 2 (60–30% HP):** Grows larger. Gains a charge attack. Small slimes become Toxic Slimes.
3. **Phase 3 (30–0% HP):** Goes berserk. Room fills with slime puddles. Rapid slam attacks.

**Rewards:**
- 🟡 Large gold drop
- 📦 Rare loot chest (equipment)
- 👑 **The Slime Crown** — legendary cosmetic headpiece (dripping green crown, wearable by player character)
- 🔑 Key to Zone 2 (Underground Crypts)

---

## 🔨 Crafting System *(Later Phase)*

> This system will be designed and built after the core city, housing, social, and dungeon systems are complete.

**Planned features:**
- Gather materials from dungeons and city
- Craft weapons, armour, potions, and furniture
- Crafting stations in player's home (workbench, alchemy table, sewing kit)
- Recipe discovery through exploration and NPC gifts

---

## 🛒 Economy & Shops

- Player earns **Gold Coins** from dungeon loot, quests, and selling items
- **Downtown Market** — general supplies, food, potions
- **Clothier** — clothing and accessories
- **Furniture Shop** — home décor and furniture
- **Weapon & Armour Shop** — combat gear
- **Black Market** *(The Slums, night only)* — rare and unusual items

---

## 📅 Development Phases

See [TODO.md](./TODO.md) for the full task breakdown, and [ROADMAP.md](./ROADMAP.md) for the scoped milestones (**v0.5 Vertical Slice** and **v1.0 Launch**), risk register, and cut list. This document describes the full vision; ROADMAP.md describes what is actually being built and in what order. See [BALANCE.md](./BALANCE.md) for the level 1–30 XP curve, stat growth, Slime King boss stats, gold economy, Sewer loot tables, and the four class skill trees.

| Phase | Focus |
|-------|-------|
| 0 | Prototype — player movement, city map, basic camera |
| 1 | Character creation |
| 2 | City & NPC system |
| 3 | Housing system |
| 4 | Combat system |
| 5 | Dungeon Zone 1 — The Sewers |
| 6 | Boss — The Slime King |
| 7 | Economy & shops |
| 8 | Polish & balance |
| 9 | Crafting system |

---

*Moonlight City — where adventure lives around every corner.* 🌙
