# 🏗️ Moonlight City — Architecture Blueprint

> Companion to [DESIGN.md](./DESIGN.md), [ROADMAP.md](./ROADMAP.md), [STORY.md](./STORY.md), and [BALANCE.md](./BALANCE.md).  
> This document describes **how the single-file game should be organised in code** through **v0.5 Demo Day**.

---

## 🌙 Architecture Rules

- **One code file:** all runtime code stays in `game.html`.
- **No build step:** open in a browser and play.
- **Logical modules, not JS modules:** use big comment banners inside one `<script>`.
- **Plain objects first:** prefer data objects + functions over classes.
- **Always playable:** refactors must keep the game runnable at the end of a session.
- **Child-readable:** section names, constants, and data tables should be easy to scan.

Suggested banner style:

```js
// ==================================================
// === SAVE SYSTEM ==================================
// ==================================================
```

---

## 📦 1. File Structure Inside `game.html`

The current prototype is ~681 lines. By v0.5, expect roughly **2,800–4,000 lines** in the single script.  
That is still manageable **if the reading order is strict**.

### Recommended section order

| Order | Section banner | What lives here | Approx. line budget |
|---|---|---|---:|
| 1 | `BOOTSTRAP` | IIFE start, canvas/context lookup, resize hooks, startup call | 40–80 |
| 2 | `CONSTANTS & ENUMS` | tile sizes, scene IDs, item types, colors, tuning numbers, schema version | 120–220 |
| 3 | `UTILITY HELPERS` | `clamp`, `lerp`, `rectContains`, RNG/noise, deep clone, text helpers | 120–220 |
| 4 | `SAVE SYSTEM` | default save object, load/save, migrate, autosave timer | 120–220 |
| 5 | `CONTENT DATA` | item defs, furniture defs, NPC defs, dialogue data, floor layouts, class defs | 250–500 |
| 6 | `INPUT SYSTEM` | keyboard, mouse, click state, `justPressed`, coordinate conversion | 120–220 |
| 7 | `AUDIO SYSTEM` | sound registry, music loop, mute/volume helpers, unlock-on-first-input | 80–160 |
| 8 | `GLOBAL GAME STATE` | master `game` object, scene stack, timers, camera, screen shake | 120–220 |
| 9 | `MAP / WORLD BUILDERS` | city map build, house room data, dungeon floor loaders | 180–320 |
| 10 | `ENTITY FACTORIES` | `createPlayer`, `createEnemy`, `createNpc`, `createDamageNumber` | 120–220 |
| 11 | `SIMULATION SYSTEMS` | movement, collision, combat, AI, loot, dialogue, relationship changes, inventory ops, furniture ops | 450–800 |
| 12 | `SCENES` | `TITLE`, `CHARACTER_CREATION`, `CITY`, `HOUSE_INTERIOR`, `DUNGEON`, overlays | 500–900 |
| 13 | `RENDERING` | tiles, sprites, entities, UI widgets, scene overlays, HUD | 450–800 |
| 14 | `MAIN LOOP` | update/draw dispatch, dt clamp, scene routing, autosave checks | 80–140 |
| 15 | `STARTUP` | build data, load save, choose initial scene, `requestAnimationFrame` | 40–80 |

### Dependency order

Read top to bottom:

1. **constants**
2. **helpers**
3. **save + content**
4. **input + audio**
5. **global state**
6. **world/entity creation**
7. **simulation systems**
8. **scenes**
9. **rendering**
10. **main loop/startup**

### Practical rule for editing

- **Parent-friendly engine sections:** save system, collision, AI, scene switching, rendering pipeline.
- **Child-friendly content sections:** dialogue tables, item lists, furniture lists, floor layouts, NPC names/positions, colour palettes.

---

## 🎬 2. Scene / State Machine

Use **one active base scene** plus **zero or more overlays**.

```js
const SCENE = {
  TITLE: "TITLE",
  CHARACTER_CREATION: "CHARACTER_CREATION",
  CITY: "CITY",
  HOUSE_INTERIOR: "HOUSE_INTERIOR",
  DUNGEON: "DUNGEON",
  DIALOGUE: "DIALOGUE",
  INVENTORY: "INVENTORY",
  PAUSE: "PAUSE"
};
```

```js
const game = {
  activeScene: SCENE.TITLE,
  overlayStack: [], // e.g. ["DIALOGUE"], ["INVENTORY"], ["PAUSE"]
  previousBaseScene: null,
  transition: { active: false, type: "fade", t: 0, nextScene: null },
  saveSlot: null,
  player: null,
  city: null,
  house: null,
  dungeon: null
};
```

### Scene rules

- **Base scenes** own world simulation: `TITLE`, `CHARACTER_CREATION`, `CITY`, `HOUSE_INTERIOR`, `DUNGEON`
- **Overlay scenes** pause or partly pause the base scene: `DIALOGUE`, `INVENTORY`, `PAUSE`
- Only the **top-most overlay** receives input.
- When an overlay is open, the base scene still renders underneath.

### Scene contract

Each scene should be a plain object with the same shape:

```js
const Scenes = {
  TITLE: {
    enter(payload) {},
    update(dt) {},
    render() {},
    onKeyDown(code) {},
    onClick(pointer) {},
    exit() {}
  }
};
```

### Base scenes

#### `TITLE`
- **Owns state:** selected button, animated background timer, whether a valid save exists
- **Renders:** logo, Start, Continue, Credits, version text
- **Handles input:** arrows/WASD or mouse for button selection, Enter/click to confirm
- **Transitions:**
  - `Start` → `CHARACTER_CREATION` if no save
  - `Continue` → restore save, then `CITY` or `HOUSE_INTERIOR`/`DUNGEON` if that is where the save left off

#### `CHARACTER_CREATION`
- **Owns state:** temp character draft `{name, skin, hairStyle, hairColour, outfit, classId}`
- **Renders:** preview sprite, option rows, name entry, confirm button
- **Handles input:** keyboard for text entry, left/right/click for options, Enter on confirm
- **Transitions:**
  - `Confirm` → write new save → spawn player → `CITY`
  - `Back` → `TITLE`

#### `CITY`
- **Owns state:** city tile map, camera target, NPC list, entrance triggers, city clock, ambient particles
- **Renders:** city map, NPCs, player, HUD, entrance markers
- **Handles input:** movement, attack, dash, interact, click NPC, click sewer/house trigger, open inventory, pause
- **Transitions:**
  - enter sewer trigger → fade → `DUNGEON`
  - enter house trigger → fade → `HOUSE_INTERIOR`
  - click NPC → push `DIALOGUE`
  - inventory key/shop click → push `INVENTORY`
  - Escape → push `PAUSE`

#### `HOUSE_INTERIOR`
- **Owns state:** room colours, placement mode, grid occupancy, owned furniture, chest/home banking interactions
- **Renders:** room, grid overlay, placed furniture, furniture palette, wall/floor swatches
- **Handles input:** movement near door, mouse placement/removal, inventory/shop opening, pause
- **Transitions:**
  - step on exit tile → `CITY`
  - choose furniture/shop storage → `INVENTORY`
  - Escape → `PAUSE`

#### `DUNGEON`
- **Owns state:** current floor, floor layout data, enemy list, loot drops, breakables, stairs unlocked flag, combat effects
- **Renders:** dungeon tiles, hazards, enemies, loot, combat UI, floor label
- **Handles input:** movement, attack, dash, pickup, pause, optional loot click if used
- **Transitions:**
  - cleared stairs → next dungeon floor
  - death → fade → respawn in city safe point
  - return/home trigger later if added → `CITY`
  - inventory key → push `INVENTORY`
  - Escape → `PAUSE`

### Overlay scenes

#### `DIALOGUE`
- **Owns state:** target NPC id, current topic list, current line index, pending relationship gain
- **Renders:** speech box, NPC portrait/sprite, topic buttons, relationship hearts/level
- **Handles input:** click topic, advance line, close
- **Transitions:**
  - close → pop overlay back to underlying scene
  - shop topic with Mara → swap to `INVENTORY` in shop mode

#### `INVENTORY`
- **Owns state:** mode (`inventory`, `shopBuy`, `shopSell`, `furniturePick`), cursor slot, selected item, tooltip text
- **Renders:** slot grid, item quantities, gold totals, shop stock, action buttons
- **Handles input:** arrows/mouse, confirm, consume, buy, sell, close
- **Transitions:**
  - close → pop overlay
  - furniture pick in house → return selected furniture id to `HOUSE_INTERIOR`

#### `PAUSE`
- **Owns state:** selected menu row
- **Renders:** dim screen, Resume, Save & Title, Quit to Title
- **Handles input:** arrows/mouse, Enter/click, Escape to resume
- **Transitions:**
  - Resume → pop overlay
  - Save & Title → write save → `TITLE`

### Recommended transition helpers

```js
function switchScene(nextScene, payload = null) {}
function pushOverlay(sceneId, payload = null) {}
function popOverlay() {}
function beginFade(nextScene, payload = null) {}
```

---

## 💾 3. Save Data Schema

Use **one localStorage slot**, versioned from day one.

```js
const SAVE_KEY = "moonlight-city-save";
const SCHEMA_VERSION = 1;
```

### Canonical save shape

```js
{
  version: 1,
  lastScene: "CITY",
  player: {
    name: "Luna",
    skin: 1,
    hairStyle: 2,
    hairColour: 5,
    outfit: 0,
    classId: "adventurer",
    level: 1,
    xp: 0,
    stats: { STR: 5, AGI: 5, INT: 5, DEF: 5 },
    hp: 30,
    hpMax: 30,
    mp: 10,
    mpMax: 10,
    spawn: { scene: "CITY", x: 960, y: 1120 }
  },
  gold: {
    carried: 0,
    banked: 0
  },
  inventory: [
    { id: "small-potion", qty: 2 }
  ],
  dungeon: {
    currentFloor: 1,
    deepestFloorCleared: 0,
    checkpointFloor: 0,
    floorsCleared: [],
    deathCount: 0
  },
  house: {
    wallColour: 0,
    floorColour: 0,
    placedFurniture: [
      { id: "starter-bed", x: 2, y: 4, rotation: 0 }
    ]
  },
  relationships: {
    mara: 0,
    rex: 0,
    lumi: 0
  },
  time: {
    currentDay: 1,
    currentHour: 8,
    currentMinute: 0
  },
  flags: {
    bossDefeated: false,
    metMara: false,
    metRex: false,
    metLumi: false,
    unlockedHouse: true
  }
}
```

### Notes

- `lastScene` supports Continue-from-last-safe-place.
- `gold.carried` is risked on death; `gold.banked` is safe at home.
- `relationships[npcId]` should stay **0–3** for v0.5.
- `time` only advances in the city. **Do not tick city time in the dungeon.**
- `flags` is the safe place for quest/story booleans. Keep names explicit.

### Migration strategy

Never overwrite a child's save with `null` or silently discard fields.

```js
function migrateSave(rawSave) {
  let save = rawSave || createNewSave();

  if (typeof save.version !== "number") save.version = 0;

  while (save.version < SCHEMA_VERSION) {
    switch (save.version) {
      case 0:
        save.gold = save.gold || { carried: 0, banked: 0 };
        save.relationships = save.relationships || { mara: 0, rex: 0, lumi: 0 };
        save.version = 1;
        break;
      default:
        return createNewSave();
    }
  }

  return normalizeSave(save);
}
```

### Migration rules

1. **Add defaults, do not delete progress.**
2. **Normalize arrays and numbers** after migration.
3. If a save is badly broken, keep as much as possible:
   - preserve player look
   - preserve inventory
   - preserve house furniture
4. Save after migration succeeds so old versions are upgraded once.

---

## 🧍 4. Entity / Object Model

Use plain JS objects plus factory functions.

### Player

```js
{
  kind: "player",
  x: 0, y: 0,
  vx: 0, vy: 0,
  radius: 9,
  dir: "down",
  moveSpeed: 96,
  accel: 700,
  dashSpeed: 220,
  dashTimer: 0,
  attackTimer: 0,
  hurtTimer: 0,
  walkTime: 0,
  hp: 30, hpMax: 30,
  mp: 10, mpMax: 10,
  level: 1,
  xp: 0,
  stats: { STR: 5, AGI: 5, INT: 5, DEF: 5 },
  look: {
    skin: 1,
    hairStyle: 2,
    hairColour: 5,
    outfit: 0
  },
  classId: "adventurer",
  state: "idle" // idle, move, attack, dash, hurt, dead
}
```

### Enemy

```js
{
  kind: "enemy",
  type: "small-slime",
  x: 0, y: 0,
  vx: 0, vy: 0,
  radius: 10,
  hp: 14, hpMax: 14,
  aiState: "wander", // wander, chase, attack, hurt, dead
  aggroRange: 96,
  leashRange: 180,
  attackRange: 20,
  attackCooldown: 0,
  hurtTimer: 0,
  facing: "left",
  lootTableId: "slime-floor-1",
  goldDropMin: 1,
  goldDropMax: 3
}
```

### NPC

```js
{
  kind: "npc",
  id: "mara",
  x: 0, y: 0,
  radius: 10,
  facing: "down",
  dialogueId: "mara",
  relationshipLevel: 0,
  schedule: {
    daySpot: { x: 0, y: 0 },
    nightSpot: { x: 0, y: 0 }
  },
  shopId: "mara-shop"
}
```

For v0.5, schedules can stay fixed-position data even if the field already exists.

### Item definition

```js
{
  id: "small-potion",
  name: "Small Potion",
  type: "consumable", // consumable, junk, furniture, key
  icon: "potion-red",
  goldValue: 12,
  effect: { kind: "heal-hp", amount: 20 },
  stats: null
}
```

### Furniture definition

```js
{
  id: "starter-bed",
  name: "Starter Bed",
  sprite: "bed-blue",
  gridW: 2,
  gridH: 3,
  category: "floor", // floor, wall
  price: 80
}
```

### Dungeon floor

```js
{
  id: "sewer-1",
  zone: "sewers",
  floorNumber: 1,
  width: 20,
  height: 14,
  tiles: [
    "####################",
    "#..s......b......>#",
    "#..####..........##"
  ],
  spawn: { x: 2, y: 2 },
  enemies: [
    { type: "small-slime", x: 8, y: 5 },
    { type: "sewer-rat", x: 14, y: 9 }
  ],
  barrels: [
    { x: 10, y: 3 }
  ],
  cleared: false
}
```

### Recommended data split inside the single file

- `ITEM_DEFS`
- `FURNITURE_DEFS`
- `NPC_DEFS`
- `DIALOGUE_DEFS`
- `ENEMY_DEFS`
- `DUNGEON_FLOOR_DEFS`

That keeps content editable without touching engine logic.

---

## 🎨 5. Rendering Pipeline

Keep rendering deterministic and layered the same way every frame.

### Draw order

1. **Scene background**
2. **Ground tiles**  
   grass, paths, stone, water base
3. **Floor decals / hazards**  
   slime puddles, cracks, rugs, placement grid highlight
4. **Shadows**
5. **World actors sorted by Y**
   - player
   - enemies
   - NPCs
   - loot
   - barrels if meant to sit in actor layer
6. **Tall obstacles drawn in front when needed**
   - trees
   - walls
   - pillars
   - furniture fronts
7. **Combat/effect layer**
   - hit flashes
   - damage numbers
   - attack arcs
   - pickups
8. **HUD/UI**
   - HP/MP, gold, floor label, dialogue prompt
9. **Overlay scenes**
   - dialogue
   - inventory/shop
   - pause
10. **Transitions**
   - fade-to-black
   - game over wash

### Y-sort rule

The sort key is the thing's **feet/base**, not its top.

```js
drawables.sort((a, b) => a.sortY - b.sortY);
```

Examples:

- player sort key = `player.y + player.radius`
- slime sort key = `enemy.y + enemy.radius`
- NPC sort key = `npc.y + npc.radius`
- tree trunk base sort key = `tileY * TILE_SIZE + 28`

### The existing tree trick

The current prototype already does the correct trick:

- draw obstacles whose base is **behind** the player
- draw player
- draw obstacles whose base is **in front of** the player

Generalise that into a shared drawable sort system so it works for:

- trees in city
- pillars in dungeon
- tall furniture in house

### Camera / effects

- Camera follows player with lerp.
- Apply **screen shake** by offsetting camera during render only.
- Apply **hit-stop** by freezing update for a tiny timer, but still allowing render.

---

## ⌨️ 6. Input System

The prototype has a held-key map. For v0.5, upgrade to **held + justPressed + mouse**.

### Keyboard map

| Action | Keys |
|---|---|
| Move up | `W`, `ArrowUp` |
| Move down | `S`, `ArrowDown` |
| Move left | `A`, `ArrowLeft` |
| Move right | `D`, `ArrowRight` |
| Basic attack | `Z`, `Space` |
| Dash | `X`, `ShiftLeft` |
| Interact / confirm | `E`, `Enter` |
| Inventory | `I` |
| Pause / back | `Escape` |

### Input data shape

```js
const input = {
  held: Object.create(null),
  pressed: Object.create(null),
  released: Object.create(null),
  mouse: {
    x: 0, y: 0,
    worldX: 0, worldY: 0,
    down: false,
    clicked: false,
    rightClicked: false
  }
};
```

### Routing rules

1. If a top overlay exists, it gets input first.
2. Otherwise the active base scene gets input.
3. UI hit boxes beat world clicks.
4. World clicks convert canvas coordinates to world coordinates after camera/zoom.
5. Reset `pressed`, `released`, `clicked` at the end of each frame.

### Mouse / click usage

- **NPC click:** test world-space circle/rect against NPCs
- **Furniture placement:** test room grid cell under mouse
- **UI buttons:** test screen-space button rectangles
- **Right click:** cancel furniture placement or remove selected furniture

### Gamepad

Post-v0.5. Keep action names abstract now (`attack`, `dash`, `pause`) so gamepad can map in later without rewriting scene logic.

---

## 🔊 7. Audio System

Keep audio simple and reliable for a hobby browser build.

### Recommended approach

- Use a small `AudioManager` in JS.
- Use **one music channel** and **SFX by cloned audio nodes** for overlap.
- Unlock audio on the first click/key press because browsers block autoplay.

```js
const AUDIO_DEFS = {
  musicCity: { src: "audio/city-theme.mp3", loop: true, volume: 0.5 },
  footstep: { src: "audio/footstep.wav", volume: 0.25 },
  attackSwing: { src: "audio/attack-swing.wav", volume: 0.35 },
  hit: { src: "audio/hit.wav", volume: 0.4 },
  coin: { src: "audio/coin.wav", volume: 0.4 },
  door: { src: "audio/door.wav", volume: 0.4 },
  uiClick: { src: "audio/ui-click.wav", volume: 0.35 },
  buy: { src: "audio/buy.wav", volume: 0.4 },
  potion: { src: "audio/potion.wav", volume: 0.35 }
};
```

### v0.5 sound list

- Footstep
- Attack swing
- Hit connect
- Enemy hurt / splat
- Coin pickup
- Door / transition
- UI click
- Buy/sell
- Potion use

One loop is enough for v0.5:

- **City theme**

Optional if time remains:

- short dungeon ambience loop

### Music rules

- Start city music on `TITLE` or `CITY` after first interaction.
- Fade down or pause under `PAUSE`.
- Stop or switch when entering `DUNGEON` only if a second loop exists.

---

## 🧠 8. Key Algorithms

### A. Y-sort rendering

**Purpose:** fake depth in a top-down game.

1. Build a list of visible entities/obstacles with `sortY`
2. Sort ascending
3. Draw in order

Use the object's foot point, not its center, so taller sprites layer correctly.

### B. Circle-vs-AABB collision

Already used in the prototype and should stay.

1. Find the blocking tiles near the circle
2. Clamp the circle center to each tile rectangle
3. Measure distance from circle center to nearest point
4. If distance `< radius`, collision

This is ideal for:

- smooth player movement
- small slimes/rats
- simple obstacle collision

### C. Dungeon floor loader

For v0.2, floors should be **hand-designed arrays**, not generated.

```js
const DUNGEON_FLOOR_DEFS = {
  1: { ... },
  2: { ... },
  3: { ... }
};
```

Loader steps:

1. read floor definition by number
2. convert tile characters into tile IDs
3. spawn enemies/barrels/loot points
4. mark stairs as locked
5. unlock stairs when enemy count reaches zero

### D. Grid-snap furniture placement

House placement should feel exact, not floaty.

1. Convert mouse world position to room-local coordinates
2. Divide by `GRID_SIZE`
3. Floor to `(gx, gy)`
4. Check rectangle occupancy for the selected furniture footprint
5. If valid, show green highlight
6. If invalid, show red highlight
7. Click to commit `{id, x: gx, y: gy, rotation}`

Store placed furniture in save data, not as live canvas state.

### E. Dialogue tree traversal

For v0.5 keep it flat:

```js
{
  greetingByRelationship: [
    "Oh! Hello there!",
    "Back again? Good.",
    "Hey, friend!",
    "There you are — I saved the good stuff."
  ],
  topics: [
    { id: "shop", label: "Show me your shop", lines: ["Take your time!"] },
    { id: "town", label: "How's the city?", lines: ["Busy, noisy, alive."] }
  ]
}
```

Traversal:

1. choose greeting from relationship level
2. show topic buttons
3. click topic
4. play topic lines in sequence
5. optionally change relationship / flag
6. return to topic list or close

### F. NPC relationship transitions

For v0.5 relationship is a small integer `0–3`.

Recommended sources of progress:

- first talk of the day: `+1 progress point`
- completing a special topic/quest: `+1`
- gift or story event: `+1`

Then map points to visible level:

```js
0-1 => level 0
2-3 => level 1
4-5 => level 2
6+  => level 3
```

Why use hidden points under a visible 0–3 level?

- lets you reward small actions
- avoids relationship jumping too fast
- still saves to a simple visible tier if desired

---

## 🔧 9. Implementation Notes for v0.5

### Recommended master state shape

```js
const game = {
  now: 0,
  dt: 0,
  activeScene: SCENE.TITLE,
  overlayStack: [],
  saveDirty: false,
  camera: { x: 0, y: 0, shakeX: 0, shakeY: 0 },
  effects: {
    hitStopTimer: 0,
    fade: { active: false, alpha: 0, nextScene: null },
    damageNumbers: []
  },
  city: { map: [], npcs: [], triggers: [] },
  house: { width: 10, height: 8, placedFurniture: [] },
  dungeon: { floorNumber: 1, tiles: [], enemies: [], barrels: [], drops: [] },
  player: null
};
```

### Autosave moments

Autosave at safe, understandable times:

- after character creation confirm
- after buying/selling
- after placing/removing furniture
- after clearing a dungeon floor
- after scene return to city/house
- when opening pause menu

### Debug helpers worth adding

- `DEBUG.showCollision`
- `DEBUG.skipToFloor`
- `DEBUG.addGold`
- `DEBUG.unlockAllFurniture`

Keep them at the bottom of constants and guard them clearly.

---

## ✅ 10. What “Good Structure” Means for This Project

If this architecture is working well, then:

- a child can edit dialogue without touching combat logic
- a parent can refactor save/load without breaking drawing code
- new stages mostly mean **adding data**, not rewriting systems
- `game.html` is long, but **not mysterious**
- v0.5 can ship without needing classes, bundlers, or frameworks

For combat numbers and XP formulas, see [BALANCE.md §0–§2](./BALANCE.md).  
For scope guardrails and Demo Day targets, see [ROADMAP.md §1](./ROADMAP.md).  
For NPC tone and dialogue content, see [STORY.md](./STORY.md).
