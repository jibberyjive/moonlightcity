# ✅ Moonlight City — TODO List

A prioritised task list for building Moonlight City. Work through phases in order — later phases depend on earlier ones.

---

## ✅ Legend
- [ ] Not started
- [x] Done
- 🔒 Blocked (depends on another task)

---

## Phase 0 — Prototype *(In Progress)*

- [x] Set up GitHub repository
- [x] Write game design document (`DESIGN.md`)
- [x] Build basic browser-based prototype (`game.html`)
  - [x] Tile map rendering (grass, trees, water, paths, rocks, flowers)
  - [x] Player movement (WASD / Arrow Keys)
  - [x] Camera that follows player
  - [x] Basic collision detection
  - [x] Animated water tiles
  - [x] Day counter UI

---

## Phase 1 — Character Creation

- [ ] Character creation screen on game start
- [ ] Choose skin tone
- [ ] Choose hair style and colour
- [ ] Choose eye colour and shape
- [ ] Choose starting outfit (top, bottom, shoes)
- [ ] Choose accessories (hat, bag, earrings)
- [ ] Name your character
- [ ] Save character to local storage
- [ ] Display character on city map after creation
- [ ] Wardrobe system — change outfit at home

---

## Phase 2 — City & NPC System

### City Map
- [ ] Design full Moonlight City map (districts: Downtown, Residential, Docks, Slums)
- [ ] Downtown district tiles and buildings
- [ ] Residential district with housing plots
- [ ] Docks district with water and boats
- [ ] Slums district with sewer entrance
- [ ] Transition between city areas (screen/zone loading)
- [ ] Day/night lighting cycle

### NPC System
- [ ] NPC base class (name, personality, schedule, relationship)
- [ ] NPC movement along daily schedule paths
- [ ] Click/tap NPC to start conversation
- [ ] Dialogue bubble UI (GotchaLife-inspired)
- [ ] Dialogue tree system (choices that branch)
- [ ] Relationship meter per NPC (Stranger → Best Friend)
- [ ] NPCs remember relationship level across days
- [ ] Dialogue changes based on relationship level
- [ ] Dialogue changes based on time of day
- [ ] Implement initial NPC cast: Mara, Rex, Lumi, Shade, Mayor Aldric
- [ ] NPC schedules (each NPC moves to different locations through the day)

---

## Phase 3 — Housing System

- [ ] Player can purchase a housing plot (Starter Shack)
- [ ] Enter house interior (separate scene/view)
- [ ] Grid-based furniture placement system
- [ ] Rotate and remove furniture
- [ ] Wall colour customisation
- [ ] Flooring customisation
- [ ] Furniture shop in Downtown (buy tables, chairs, beds, etc.)
- [ ] Storage chest (links to player inventory)
- [ ] House exterior visible on city map
- [ ] House upgrade system (Shack → Cottage → Townhouse → Manor)
- [ ] Lighting system (candles, lanterns, magic lights)
- [ ] NPCs can visit player's home at high relationship levels

---

## Phase 4 — Combat System

- [ ] Player stats system (HP, MP, STR, AGI, INT, DEF)
- [ ] Basic attack (melee swing)
- [ ] Dodge / dash mechanic
- [ ] Skill system (class-based active abilities with cooldowns)
- [ ] Spell system (mana-cost magic attacks)
- [ ] Technique system (charged/combo special moves)
- [ ] Status effects (poison, slow, stun, burn)
- [ ] Enemy AI base class (patrol, chase, attack)
- [ ] Hit detection and damage numbers
- [ ] HP bar UI (player and enemies)
- [ ] MP bar UI
- [ ] XP gain on enemy defeat
- [ ] Level up system (stat points + skill unlock)
- [ ] Class selection at character creation
  - [ ] Blade Dancer
  - [ ] Mage
  - [ ] Guardian
  - [ ] Rogue
- [ ] Equipment system (weapon, armour, accessory slots)
- [ ] Loot drops from enemies (gold + items)

---

## Phase 5 — Dungeon: The Sewers (Floors 1–25)

- [ ] Sewer entrance in The Slums
- [ ] Dungeon floor loader (generates or loads a floor layout)
- [ ] Sewer tileset (dark stone, slime puddles, pipes, lanterns)
- [ ] Floor progression — clear enemies to unlock stairs to next floor
- [ ] Save deepest cleared floor (resume from checkpoint)
- [ ] Slime puddle hazard (slows player movement)
- [ ] Breakable barrels and crates (random loot)
- [ ] Hidden side rooms with chests
- [ ] Implement Sewer enemies:
  - [ ] Small Slime (floors 1–10)
  - [ ] Sewer Rat (floors 1–15)
  - [ ] Toxic Slime (floors 10–20, inflicts poison)
  - [ ] Pipe Spider (floors 15–25, drops from ceiling)
  - [ ] Giant Slime (floors 20–25, splits on death)
- [ ] Loot table for Sewer zone (materials, gold, equipment)
- [ ] Floor 25 unlocks the Slime King boss door

---

## Phase 6 — Boss: The Slime King 👑

- [ ] Slime King boss arena (large room, floor 25)
- [ ] Slime King sprite and animations
- [ ] Boss HP bar UI
- [ ] Phase 1 (100–60% HP): stomp, slime ball throw, spawns small slimes
- [ ] Phase 2 (60–30% HP): grows larger, gains charge attack, small slimes → Toxic Slimes
- [ ] Phase 3 (30–0% HP): berserk mode, room fills with slime puddles, rapid slams
- [ ] Boss defeat sequence (animation + music sting)
- [ ] Slime King loot drop: gold, rare chest, **Slime Crown** cosmetic
- [ ] Slime Crown wearable in character customisation
- [ ] Key to Zone 2 drops (for future dungeon expansion)
- [ ] Boss kill tracked in player save data

---

## Phase 7 — Economy & Shops

- [ ] Gold Coin currency system
- [ ] Gold display in HUD
- [ ] Shop UI (browse items, buy with gold)
- [ ] Downtown Market (potions, food, supplies)
- [ ] Clothier shop (clothing, accessories)
- [ ] Furniture shop (home décor)
- [ ] Weapon & Armour shop (combat gear)
- [ ] Black Market (The Slums, night only — rare items)
- [ ] Sell items from inventory to shops
- [ ] Quest board in city (simple fetch/kill quests for gold rewards)

---

## Phase 8 — Polish & Balance

- [ ] Sound effects (footsteps, combat hits, UI clicks, ambient city sounds)
- [ ] Background music (city theme, dungeon theme, boss theme)
- [ ] Main menu / title screen
- [ ] Settings screen (volume, controls)
- [ ] Save / load game system (local storage)
- [ ] Game over screen (return to city on player death)
- [ ] Balance pass on combat (damage, enemy HP, XP curve)
- [ ] Balance pass on economy (shop prices, loot rates)
- [ ] Mobile / touch controls support
- [ ] Accessibility options (text size, colour modes)
- [ ] Tutorial / onboarding flow for new players

---

## Phase 9 — Crafting System *(Later)*

- [ ] Design crafting recipes and material system
- [ ] Gather materials from dungeon floors
- [ ] Workbench crafting station (place in home)
- [ ] Alchemy table (potions and consumables)
- [ ] Sewing kit (clothing and accessories)
- [ ] Recipe discovery (find recipes in dungeons, buy from NPCs, get as gifts)
- [ ] Craft weapons and armour
- [ ] Craft furniture and home décor
- [ ] Craft potions and buffs

---

## 🔮 Future / Backlog

- [ ] Dungeon Zone 2 — The Underground Crypts (floors 26–50)
- [ ] Dungeon Zone 3 — The Magma Depths (floors 51–75)
- [ ] Dungeon Zone 4 — The Shadow Realm (floors 76–100)
- [ ] Dungeon Zone bosses for zones 2–4
- [ ] Multiplayer / co-op mode
- [ ] Player-to-player trading
- [ ] Guild system
- [ ] Seasonal city events and festivals
- [ ] Pet companions
- [ ] Mounts / faster city travel

---

*Last updated: August 2026*
