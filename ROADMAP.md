# 🗺️ Moonlight City — Production Roadmap & Scope Plan

> Companion document to [DESIGN.md](./DESIGN.md) and [TODO.md](./TODO.md).
> DESIGN.md describes **what the game could be**. This document describes **what we will actually build, in what order, and what we are deliberately not building.**

**Team:** 2 people (parent + child), hobby time, no deadline.
**Tech:** single-file HTML/JS/Canvas browser game.
**Primary risk:** not failure — *drift*. Passion projects rarely die from bad code; they die from a to-do list that never gets shorter.

---

## 0. The Governing Principle — The Cut Line

The current TODO list is roughly **80 tasks across 9 phases**. Realistically that is **150–250 working sessions** for a 2-person hobby team. At two sessions a week that is **2–3 years**. That is not a reason to panic — it is a reason to define *shippable slices*.

Three rules that protect this project:

1. **Always playable.** `game.html` must run and be fun-ish at the end of *every* session. Never leave the game broken between sessions — the next session's motivation depends on being able to press Play.
2. **One-in / one-out.** Any new idea added to the roadmap must displace something already on it, or it goes to the **Backlog** (idea parking lot) in TODO.md. Ideas are free; scope is not.
3. **Vertical before horizontal.** Build one thin slice of *every* pillar before building any pillar deeply. A game with 5 shallow systems is a game. A game with 1 deep system (e.g. 100 dungeon floors and nothing else) is a tech demo.

---

## 1. 🎯 Milestone v0.5 — "The Vertical Slice"

> **Goal:** The smallest build that *feels like a real game* and survives being shown to a friend without explanation or apology.

### 1.1 The definition of success

A friend sits down, is handed the keyboard, and **plays for 10 minutes without being told what to do**. They make a character, walk into the city, talk to someone, go into the sewer, kill slimes, come back with gold, buy a chair, put it in their house, and want to go back down. That's it. That's the game.

**If the 10-minute loop is fun, everything else in DESIGN.md is just more of a thing that already works. If it isn't fun, no amount of content will save it.**

### 1.2 The core loop v0.5 must close

```
Create character → Explore city → Talk to NPC → Descend into sewers
      ↑                                                    ↓
      └──── Decorate home ← Buy item ← Earn gold ← Fight & loot
```
Every arrow in that diagram must work. Nothing outside that diagram belongs in v0.5.

### 1.3 Must include (the contract)

| Area | v0.5 scope | Explicitly NOT in v0.5 |
|---|---|---|
| **Character creation** | Name, 4 skin tones, 6 hair styles, 8 hair colours, 3 outfits, 1 class. Saved to local storage. | Face sliders, body shape/height, gradients, accessories, expression styles, wardrobe changes |
| **City** | **One contiguous map**, ~64×64 tiles, visually split into Downtown / Residential / Slums. No zone loading. | The Docks, screen transitions, festivals |
| **NPCs** | **3 NPCs** (Ella, Rex, Lumi) standing in fixed spots. Click → dialogue bubble → 2–3 flat topic choices. A 3-level relationship counter that changes their greeting. | Schedules, pathing, branching trees, memory of past conversations, night-only NPCs |
| **Housing** | **One pre-owned Starter Shack**, one interior room, grid snap placement of **8 furniture items**, wall + floor colour swatches. Saves and reloads correctly. | Buying plots, upgrades/tiers, rotation, storage chests, NPC visits, exterior variations |
| **Combat** | **One class**. Move, basic attack, dash. HP/MP bars, damage numbers, death → wake up at home minus some gold. | Spells, techniques, status effects, 4 classes, equipment slots |
| **Dungeon** | **Sewer floors 1–5**, hand-designed layouts, stairs unlock when the floor is cleared. 2 enemy types (Small Slime, Sewer Rat). Breakable barrels. Deepest floor saved. | Floors 6–25, procedural generation, hazards, hidden rooms, minibosses |
| **Economy** | Gold in HUD. **One shop** (Ella's) selling 3 potions + 5 furniture items. Loot drops gold + junk-to-sell. | Multiple shops, selling UI beyond one button, quest board, black market |
| **System** | Title screen, save/load (single slot, **versioned save data**), pause, game over. | Settings, options, multiple slots, mobile controls |
| **Feel** | 6–8 sound effects + 1 music loop. Hit-stop on attacks. Screen shake on boss-ish hits. | Full soundtrack, ambient city audio |

### 1.4 Honest budget

**~30 sessions of ~90 minutes** (≈4–6 months at 2 sessions/week). Expect it to be 40. That's normal, not a failure.

### 1.5 v0.5 stepping stones (always-playable checkpoints)

| Tag | Ships when… |
|---|---|
| `v0.1` | Character creation → walk around city with a name and a look you chose |
| `v0.2` | Sewer entrance works; 3 floors; slimes that can hurt and be hurt; you can die |
| `v0.3` | Gold, loot, one shop, inventory |
| `v0.4` | House interior + furniture placement + saving |
| `v0.5` | 3 NPCs, dialogue, sound, title screen, polish pass → **DEMO DAY** |

---

## 2. 🚀 Milestone v1.0 — "Launch"

> **Goal:** A complete, satisfying, *finishable* game. A player can start it, play for 6–10 hours, beat the Slime King, and feel the story is done — even though the world could grow later.

v1.0 is **v0.5 plus depth in the same systems**, not new systems. The one exception is the boss, which is the game's ending.

### 2.1 v1.0 feature set (the minimum for "complete")

**Character & progression**
- **2 classes** (Blade Dancer, Mage) — not 4. Chosen at creation, each with 3 unlockable skills.
- Levels 1–20 with a tuned XP curve; stat points on level up.
- Equipment: **3 slots only** (weapon, armour, accessory).
- Wardrobe at home; ~20 clothing items obtainable through play.

**City**
- 4 districts including The Docks, connected on one map or with simple fade transitions.
- **Day/night cycle** as a colour/lighting overlay + "shops closed at night" rule. (Cheap, huge atmosphere payoff.)
- 5–7 NPCs with relationship levels (Stranger → Best Friend), 3 dialogue sets each keyed to relationship level, plus one time-of-day variant.
- **Light schedules:** each NPC has 2 positions (day spot, night spot). Not pathfinding — teleport at dawn/dusk when off-screen.
- Quest board with ~10 simple fetch/kill quests.

**Housing**
- **2 tiers** (Shack → Cottage). Buy the upgrade with gold — this is a major gold sink and a real goal.
- ~40 furniture items, rotation, storage chest, lighting objects.
- One NPC visit event at Best Friend level (scripted, not systemic).

**Dungeon & boss**
- **Sewers floors 1–25**, built from a library of ~15 hand-made room templates stitched procedurally, with a difficulty curve per band.
- All 5 sewer enemy types + Giant Slime miniboss.
- Hazards, breakables, hidden chest rooms.
- **The Slime King, 3 phases**, boss HP bar, defeat sequence, Slime Crown cosmetic, gold + rare chest.
- Post-boss: a locked Zone 2 door with a "Coming soon" sign. **This is the ending.**

**Economy**
- 4 shops (Market, Clothier, Furniture, Weapons & Armour). Buy + sell.
- Balanced gold curve: house upgrade should take ~40% of expected v1.0 earnings.

**Polish**
- Full save/load with migration, settings screen, tutorial prompts in the first 5 minutes, balance passes on combat and economy, mobile/touch controls (so it can be shown on a tablet), text-size accessibility option.

### 2.2 Explicitly deferred past v1.0

Crafting (all of Phase 9), dungeon zones 2–4, the black market, seasonal events, multiplayer, trading, guilds, pets, mounts, house tiers 3–4, classes 3–4, techniques as a separate system.

### 2.3 Honest budget

**~70–90 further sessions** after v0.5 (≈9–15 months at 2/week). Total project: **~18 months to 2 years of relaxed pace**. Say it out loud now so nobody feels behind later.

### 2.4 v1.0 stepping stones

| Tag | Theme |
|---|---|
| `v0.6` | Dungeon depth: floors 1–15, room templates, all enemy types, hazards |
| `v0.7` | Combat depth: 2nd class, skills, equipment, level curve |
| `v0.8` | City & social depth: 4 districts, day/night, full NPC cast, quest board, all shops |
| `v0.9` | Housing depth: tier 2, 40 furniture items, rotation, storage |
| `v1.0` | Floors 16–25 + Slime King + tutorial + balance + audio + touch controls |

---

## 3. ⚠️ Top 5 Risks (ranked by "most likely to kill the project")

### Risk 1 — The 100-floor dungeon (content treadmill)
**Why it kills projects:** 100 hand-made floors is 100 units of work with no visible progress between them. Even 25 floors is 25. The team burns out somewhere around floor 7 and the project quietly stops.

**Mitigation**
- **Never hand-author floors.** Build a **room-template library** (~15 hand-made rooms) plus a stitcher that lays out 4–8 rooms per floor with a seeded RNG. 15 rooms → 25 floors that feel varied.
- Make floors **data, not code**: a floor is `{ zone, depth, enemyBudget, roomTags, lootTier }`. Difficulty is a formula, not 25 tuning sessions.
- **Ship at 25.** Zone 2 is a post-1.0 expansion, and the locked door is a *feature* (it promises more), not a failure.
- Deliberately allow **fast descent**: floors should take 60–120 seconds. Twenty-five short floors > five long ones.

### Risk 2 — The Sims-style housing editor
**Why it kills projects:** It looks like "place a chair" and is actually grid occupancy, multi-tile footprints, rotation states, draw-order/depth sorting, wall-vs-floor item classes, collision, undo, and save serialisation. It is easily the most complex *engineering* task in the whole design, and it's invisible progress for weeks.

**Mitigation**
- **v0.5: strict 1×1 grid, no rotation, one room, 8 items, delete = right-click.** Nothing else. Prove the save/load round-trip early.
- Store furniture as a flat array `{ id, gx, gy, rot }` — no object graph, no nesting. Rotation is added later as a field that already exists but is always 0.
- Separate **wall items** and **floor items** into two arrays from day one; retrofitting that later is painful.
- Wall/floor colour customisation is a **swatch index**, not a colour picker. Cheap, and 90% of the joy.
- Budget it as its own mini-project with its own demo day — it is not "one task".

### Risk 3 — Dialogue & NPC content volume
**Why it kills projects:** Branching dialogue trees × 5 NPCs × 4 relationship levels × time of day = hundreds of lines of writing plus a tree editor plus a memory/flag system. Writing is fun for one evening and gruelling by the third.

**Mitigation**
- **No deep trees.** Use a flat **topic menu**: click NPC → 2–4 topics → single response + optional relationship delta. This gets ~90% of the GotchaLife feeling for ~10% of the work.
- Dialogue lives in **one big data object** (or a separate `dialogue.js` data file), not scattered in code. **This is the child's job** — she can write and edit lines without touching engine code, and it becomes her authored part of the game.
- "NPCs remember" = a **flag set** (`met`, `gaveGift`, `beatBoss`) checked in a single line-selection function. Not a memory system.
- Content target for v1.0: **~25 lines per NPC.** Write them in a spreadsheet, paste them in.

### Risk 4 — Combat feel and balance
**Why it kills projects:** DESIGN.md lists skills *and* spells *and* techniques *and* status effects *and* 4 classes — that's four overlapping systems and 12+ abilities before anything is fun. Meanwhile, combat that doesn't *feel* good makes 25 floors feel like a chore, and no amount of content fixes it.

**Mitigation**
- **Collapse skills / spells / techniques into ONE system:** an ability with `{ cost, cooldown, damage, shape, effect }`. Class flavour is data, not architecture.
- Ship **one class** in v0.5, **two** in v1.0. A second class is a content addition once the ability system exists.
- **Spend a whole session on "feel" before adding content:** hit-stop (~80ms freeze on hit), knockback, damage numbers, screen shake, a satisfying hit sound, invulnerability frames after taking damage. This session is worth more than 10 new enemy types.
- Balance via **formulas with constants at the top of the file** (`ENEMY_HP_BASE`, `ENEMY_HP_GROWTH`), so a balance pass is 6 numbers, not 60 edits.
- Status effects: **poison only** for v1.0. Slow/stun/burn are Backlog.

### Risk 5 — Single-file codebase entropy + save-data breakage
**Why it kills projects:** `game.html` is ~680 lines today. v1.0 is 6,000–10,000. In one file with two people editing it, that means merge conflicts, fear of touching things, and eventually "I don't want to open that file". Worse: every schema change silently destroys the child's saved character and house — which is a *motivation* catastrophe, not just a bug.

**Mitigation**
- **Split at the v0.5 boundary** into a few ES modules (`engine.js`, `world.js`, `combat.js`, `ui.js`, `data/*.js`) loaded via `<script type="module">`. Still no build step, still opens in a browser, still "one project you can double-click".
- Keep all tunable numbers and all content in `data/` files. **Rule: the child edits `data/`, the parent edits engine files.** This nearly eliminates merge conflicts and gives clean ownership.
- **Version the save from today:** `{ version: 1, ... }` plus a `migrate()` function. Never break her save file. Ever.
- Two-person git hygiene: short-lived branches, work on different files in a session, push at the end of every session.

### Honourable mentions (real, but manageable)
- **Art pipeline.** Don't start a pixel-art pipeline. Keep drawing sprites procedurally with Canvas primitives (the current code already does this well) — it's fast, consistent, resolution-independent, and the child can change colours and shapes by editing numbers.
- **Motivation drift.** The real deadline is the child's interest. Mitigate with **Demo Day every ~6 sessions** — show a family member or friend, watch them play, write down 3 things. External eyes are the highest-value fuel this project has.

---

## 4. 🗓️ Development Rhythm (parent + child)

### 4.1 The weekly shape — 2 sessions

| Session | Length | Who | Focus |
|---|---|---|---|
| **Midweek — "Design & Play"** | 45–60 min | **Together** | Play the current build for 10 min. Write 3 bugs + 1 idea on the whiteboard. Pick *one* thing for the weekend. Draw / decide / name things. No coding pressure. |
| **Weekend — "Build"** | 90–120 min | **Together, then split** | First 30 min: pair on the fun visible part. Then split — child on content/data, parent on plumbing. Last 10 min: **play the result together** and commit. |
| **Any time — "Solo plumbing"** | parent's own time | Parent | Refactors, save migration, module splits, bug fixes. Never do this *during* shared time — it's boring to watch. |

### 4.2 What to do together vs separately

**Together (high energy, high joy)**
- Playing and bug-hunting (the child is the better tester — she will find everything)
- Naming NPCs, items, enemies; writing their personalities
- Designing the city layout and dungeon rooms **on graph paper first**
- Choosing colour palettes and sprite shapes
- Tuning numbers live (change a value, reload, feel the difference) — this is the single most rewarding shared activity in game dev
- Demo Day

**Child, separately (real ownership, no JS required)**
- Writing dialogue lines in a spreadsheet → pasted into `data/dialogue.js`
- Designing room/house layouts on graph paper or in a simple tile-grid text format
- Naming and pricing shop items in `data/items.js`
- Keeping the **bug list** and the **ideas parking lot**
- Deciding what the Slime Crown looks like 👑

**Parent, separately (the invisible 60%)**
- Engine, save/load, collision, rendering, the housing grid, refactoring, git

### 4.3 Session rituals that keep it alive

1. **Start by playing.** Always. Two minutes. It reminds you what you're making.
2. **One goal per session, written down first.** "Slimes drop gold" — not "work on combat".
3. **End playable and committed.** Never end mid-refactor.
4. **Session log:** one line in `TODO.md` or a `CHANGELOG.md` — "Session 14: slimes split when they die 🎉". Momentum you can *see* is what carries a 2-year project.
5. **Demo Day every ~6 sessions.** Real audience, real reactions.
6. **The Idea Jar:** every idea gets written down and *praised*, then filed in Backlog. Never say no to an idea — say "that's a great v2 idea, let's write it down". This is how you protect scope without deflating a kid.
7. **Permission to skip a week.** A hobby project you can pause is a hobby project you can return to.

---

## 5. ✂️ Simplify or Cut (without hurting the core)

### Cut entirely from v1.0 (move to Backlog)
| Feature | Why it's safe to cut |
|---|---|
| **Crafting system (Phase 9)** | Overlaps with shops as a gear source. Adds materials, recipes, stations, discovery, 3 UIs. Zero of the core loop depends on it. **Biggest single saving on the board.** |
| **Dungeon zones 2–4 (floors 26–100)** | 25 floors + a boss is a complete game. A locked door is a better ending than an unfinished zone. |
| **Classes 3 & 4 (Guardian, Rogue)** | Two classes already give replay value. Adding classes later is cheap once abilities are data. |
| **Techniques as a separate system** | Fold into skills. Pure architecture savings, zero player-facing loss. |
| **Housing tiers 3 & 4 (Townhouse, Manor)** | Multi-room houses mean room transitions, exterior variants, and 3× furniture demand. Shack → Cottage already delivers the upgrade fantasy. |
| **Black Market** | A fifth shop UI with a night-only gate for flavour only. Merge the fun items into the Slums or the Market. |
| **Seasonal events, multiplayer, trading, guilds, pets, mounts** | Correctly already in Backlog. Keep them there. |
| **Status effects other than poison** | Each one is UI + timer + visual + balance. Poison alone proves the concept. |

### Simplify (keep the feeling, drop the cost)
| Feature | Full vision | Simplified version |
|---|---|---|
| **NPC schedules** | Pathfinding along daily routines | 2 fixed positions (day/night), swapped off-screen at dawn/dusk |
| **Dialogue trees** | Branching choices with consequences | Flat topic menu + relationship delta + flag checks |
| **Zone transitions** | Separate loaded areas | One larger map; districts are just visual regions (also better for exploration) |
| **Day/night cycle** | Full lighting engine | A tinted overlay + a few lit windows + "shops closed" rule |
| **Character creation face options** | Nose/mouth/freckles/blush/body/height | Skin, hair style, hair colour, eyes, outfit. Nobody will miss noses. |
| **Furniture rotation** | 4-way rotation from day one | Ship 1×1 non-rotating; add rotation in v0.9 when the data field is already there |
| **Dungeon floors** | Hand-crafted | Room-template stitcher with hand-made rooms |
| **Loot tables** | Per-enemy tables | Per-*zone-band* tables + a small per-enemy signature drop |
| **Quests** | Rich quest system | 3 templates (kill N, fetch X, reach floor N) filled from data |
| **Enemy AI** | Patrol/chase/attack state machines per enemy | One AI with tunable constants (`aggroRange`, `speed`, `attackRate`, `telegraphTime`) |
| **Sound** | Full soundtrack + ambience | 8 SFX + 3 music loops (city / dungeon / boss). Free CC0 assets are fine. |

### Do NOT cut (these *are* the game)
- Character creation (it's the first 3 minutes and the emotional hook)
- Housing decoration (a core pillar and, for a child collaborator, usually **the** pillar)
- Talking to NPCs
- The gold → shop → decorate loop
- The Slime King (a game needs an ending)
- Game feel: hit-stop, sound, screen shake, damage numbers

---

## 6. ✅ Definition of Done (per feature)

A feature is not done when it works on the parent's machine. It's done when:

1. It works from a **fresh save** and from an **existing save** (migration tested).
2. It survives a **page reload** mid-play.
3. The **child played it and understood it without being told how.**
4. It has at least one sound or visual response to player input.
5. `TODO.md` is ticked and the change is committed and pushed.

---

## 7. 📊 The Only Two Numbers That Matter

- **Sessions since last playable build:** target **0**. If this is ever >2, stop adding and fix.
- **Sessions since last Demo Day:** target **≤6**.

Everything else — task counts, phases, percentages — is decoration.

---

*Ship the slice. The city can keep growing forever after that.* 🌙
