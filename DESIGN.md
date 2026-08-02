# 🌙 Moonlight City — Game Design Document

> A cozy 2D city life & adventure game inspired by Avatar World, The Sims, GotchaLife, and classic dungeon crawlers.

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
| Name | Personality | Location | Hook |
|------|-------------|----------|------|
| Mara | Cheerful shopkeeper | Downtown market | Sells potions |
| Rex | Gruff ex-adventurer | The Slums tavern | Hints about sewers |
| Lumi | Shy artist | Residential park | Gives decoration items |
| Shade | Mysterious figure | Docks at night | Dungeon lore |
| Mayor Aldric | Pompous | City Hall | City quests |

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

See [TODO.md](./TODO.md) for the full task breakdown.

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
