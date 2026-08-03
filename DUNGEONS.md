# 🗺️ Moonlight City — Dungeon Layout & Encounter Design

> Companion to [DESIGN.md](./DESIGN.md) and [BALANCE.md](./BALANCE.md). Defines the floor data shape for v0.5 hand-authored Sewer floors and the template library for v1.0 stitched floors.

## 1. Dungeon Tile Vocabulary

| Tile | ID | Canvas look | Blocks movement? | Special behaviour |
| --- | --- | --- | --- | --- |
| `DTILE.FLOOR` | 0 | Dark wet stone with soft blue-gray highlights and occasional slime sheen. | No | Base walkable tile. |
| `DTILE.WALL` | 1 | Chunky sewer masonry with moss, cracks, and pipe shadows. | Yes | Opaque; hides secret seams unless replaced by SECRET_WALL. |
| `DTILE.DOOR_LOCKED` | 2 | Iron-banded hatch with a red padlock glow. | Yes | Unlocks from clear logic, switch logic, or scripted events. |
| `DTILE.DOOR_OPEN` | 3 | Door slid aside with warm lantern spill on the floor. | No | Marks an already-open room link. |
| `DTILE.STAIRS_DOWN` | 4 | Stone steps with a sleepy moon sigil and a faint chain overlay. | Yes until clear | Visible from the start; becomes STAIRS_UNLOCKED when all enemies die. |
| `DTILE.STAIRS_UNLOCKED` | 5 | Same stair tile, now bright, sparkling, and easy to read. | No | Lets the player descend and save deepest-floor progress. |
| `DTILE.BARREL` | 6 | Round damp barrel with crooked hoops and drippy wood grain. | Yes | 1 HP prop; breaks for gold, materials, or tiny heals. |
| `DTILE.CHEST` | 7 | Short brass-bound chest with a toy-like moon latch. | Yes | Opens once; usually keyed to band loot tables. |
| `DTILE.SLIME_PUDDLE` | 8 | Goopy green-blue puddle with animated bubbles and glossy edges. | No | Applies slow; safe for kids, gross in a funny way. |
| `DTILE.PIPE_STEAM` | 9 | Bent pipe nozzle with puffs of white steam and water sparkle. | No | Periodic hazard; bursts on a timer. |
| `DTILE.DARKNESS` | 10 | Extra-dim tile vignette with only lantern-edge visibility. | No | Shrinks sight radius and hides telegraphs outside the light cone. |
| `DTILE.DRIP_TRAP` | 11 | Ceiling drip marker with a wobbling puddle ripple underneath. | No | Telegraphed splash hit if the player lingers. |
| `DTILE.SECRET_WALL` | 12 | Looks like WALL, but with one hairline crack and damp airflow. | Yes | Breaks in 1 hit to reveal a hidden room. |
| `DTILE.LANTERN` | 13 | Small hanging lantern with a cozy amber halo. | No | Decorative light source; expands local sight radius. |
| `DTILE.SEWER_GRATE` | 14 | Metal floor grate with moonwater glint and gurgle bubbles. | No | Pure decoration; can tint ambient sound. |

### 1.1 Hazard rules

- **Slime puddle (`DTILE.SLIME_PUDDLE`)** — slows move speed by **30%** for players and **15%** for Sewer Rats; Small Slimes ignore the slow. No damage.
- **Steam jet (`DTILE.PIPE_STEAM`)** — cycles **1.2 s on / 1.8 s off**. Standing in the burst deals **6 + 2 × band** damage every 0.6 s.
- **Darkness zone (`DTILE.DARKNESS`)** — cuts local vision to **5 tiles** unless a `LANTERN` is nearby. Enemy aggro still works normally once they spot the player.
- **Drip trap (`DTILE.DRIP_TRAP`)** — 0.6 s telegraph (ceiling wobble + drip sound), then splash for **4 + 2 × band** damage and a **20% slow for 1 s**.
- **Secret wall (`DTILE.SECRET_WALL`)** — inherits the `WALL` look until hit. Breaks in **1 hit** and permanently converts to `DTILE.FLOOR`.
- **Lantern (`DTILE.LANTERN`)** — decorative in v0.5, functional in v1.0: grants a **4-tile warm light radius** and cancels `DARKNESS` in that radius.
- **Sewer grate (`DTILE.SEWER_GRATE`)** — decorative floor variant; use to make walkable space feel less flat without adding collision.

## 2. Floor Data Format

```js
const DTILE = {
  FLOOR: 0,
  WALL: 1,
  DOOR_LOCKED: 2,
  DOOR_OPEN: 3,
  STAIRS_DOWN: 4,
  STAIRS_UNLOCKED: 5,
  BARREL: 6,
  CHEST: 7,
  SLIME_PUDDLE: 8,
  PIPE_STEAM: 9,
  DARKNESS: 10,
  DRIP_TRAP: 11,
  SECRET_WALL: 12,
  LANTERN: 13,
  SEWER_GRATE: 14,
};

/** row-major tiles[y * w + x] */
const floor = {
  id: 6,
  seed: 6006,
  handcrafted: false,
  w: 20,
  h: 15,
  tiles: [],
  enemies: [
    {
      id: 'f6_e0',
      type: 'small_slime',
      x: 10,
      y: 6,
      ai: 'patrol',              // 'stationary' | 'patrol' | 'ambush' | 'elite'
      facing: 'w',
      patrol: [{ x: 10, y: 6 }, { x: 13, y: 6 }],
      leash: 5,
      levelOffset: 0,
      spawnRoom: 'small-arena',
      lootTable: 'band2-enemy'
    }
  ],
  chests: [
    {
      id: 'f6_chest_0',
      x: 15,
      y: 4,
      lootTable: 'band2-chest',
      hidden: false,
      requiresClear: true,
      key: null,
      contentsHint: 'gear-roll'
    }
  ],
  barrels: [
    { x: 4, y: 8, lootTable: 'band2-barrel', guaranteed: false }
  ],
  hazards: [
    { type: 'steam', x: 9, y: 5, cycleOn: 1.2, cycleOff: 1.8 },
    { type: 'darkness', x: 3, y: 3, w: 5, h: 4 }
  ],
  spawns: {
    player: { x: 1, y: 7 },
    stairs: { x: 18, y: 7 }
  },
  sideRoom: null,
  metadata: {
    band: 2,
    theme: 'sewers',
    templateChain: ['entry-room', 'straight-corridor-h', 'small-arena', 'exit-room'],
    oohMoment: 'secret-room',
    miniBoss: null
  }
};
```

### Descriptor notes
- `tiles` is always the source of truth for collision, rendering, and stairs lock state.
- `enemies` stores **spawn intent**, not live runtime state; enemy HP, cooldowns, and current target belong in combat entities at runtime.
- `spawnRoom` and `templateChain` let procedural floors cross-reference the room catalogue in §4 without hard-coding encounter logic into rendering code.
- `sideRoom` is optional and is used for both handcrafted secrets (floor 4) and procedural branches.
- `seed` must be deterministic per floor number so a child can replay a favourite layout and memorise it.

## 3. Hand-Designed Floors 1–5

**Grid legend:** `.` floor · `#` wall · `B` barrel · `C` chest · `E` enemy spawn · `S` locked stairs location · `P` player spawn · `~` slime puddle · `X` secret wall

### Floor 1 — Tutorial Drain Hall

**Theme:** Tutorial — wide open, generous sight lines, first chest tucked into a safe nook.

```text
####################
#P.................#
#.....E............#
#....B.............#
#..................#
#........E.........#
#..E...............#
#............###...#
#............#C#...#
#............###...#
#..............E...#
#.................S#
#......E.......E...#
#..................#
####################
```

```js
const FLOOR_01_TILES = [
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 7, 1, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 4, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
];
```

**Enemy placements**
- `small_slime` at `(6, 2)` — stationary; stationary
- `small_slime` at `(9, 5)` — patrol; patrol (9,5) → (11,5)
- `sewer_rat` at `(3, 6)` — patrol; patrol (3,6) → (6,6)
- `small_slime` at `(15, 10)` — stationary; stationary
- `small_slime` at `(7, 12)` — patrol; patrol (7,12) → (10,12)
- `sewer_rat` at `(15, 12)` — patrol; patrol (15,12) → (17,12)

**Chest descriptors**
- `f1_nook` at `(14, 8)` — `band1-chest`, hidden=false, requiresClear=false, hint: small potion + coins

**Barrels**
- barrel at `(5, 3)` — `band1-barrel`, guaranteed=true

**Ooh! moment:** An obvious little chest nook teaches that poking around off the main line pays off.

**Atmosphere note:** A broad, echoey starter hall with lantern-light bouncing off puddles instead of anything scary.

### Floor 2 — Narrow Pipes

**Theme:** Narrow pipes — long corridors, patrol lanes, first slime puddles.

```text
####################
#P....#......#....S#
#.##..#..~~..#..##.#
#.....#......#.....#
#.###..####..###...#
#...E..#....#..E...#
#.###.##.##.##.###.#
#E..#....##....#..E#
#.#.#.##.##.##.#.#.#
#...#....E.....#...#
#.###.######.###.#.#
#E..#......#...#..E#
#...#..~~..#..E....#
#..................#
####################
```

```js
const FLOOR_02_TILES = [
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
  1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 4, 1,
  1, 0, 1, 1, 0, 0, 1, 0, 0, 8, 8, 0, 0, 1, 0, 0, 1, 1, 0, 1,
  1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1,
  1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 1, 0, 1,
  1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1,
  1, 0, 1, 0, 1, 0, 1, 1, 0, 1, 1, 0, 1, 1, 0, 1, 0, 1, 0, 1,
  1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1,
  1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1,
  1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1,
  1, 0, 0, 0, 1, 0, 0, 8, 8, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
];
```

**Enemy placements**
- `sewer_rat` at `(4, 5)` — patrol; patrol (3,5) → (5,5)
- `small_slime` at `(14, 5)` — stationary; stationary
- `sewer_rat` at `(1, 7)` — patrol; patrol (1,7) → (3,7)
- `sewer_rat` at `(17, 7)` — patrol; patrol (15,7) → (17,7)
- `small_slime` at `(9, 9)` — stationary; stationary
- `sewer_rat` at `(1, 11)` — patrol; patrol (1,11) → (2,11)
- `small_slime` at `(17, 11)` — stationary; stationary
- `sewer_rat` at `(14, 12)` — patrol; patrol (13,12) → (16,12)

**Chest descriptors**
- none

**Barrels**
- none

**Ooh! moment:** The player learns to read lane danger when a rat patrols across the first slime puddle stretch.

**Atmosphere note:** It feels like wriggling through the city’s plumbing: cramped, drippy, and adventurous rather than mean.

### Floor 3 — The Flooded Room

**Theme:** Flooded room — a central slime basin with clustered enemies.

```text
####################
#P.......####......#
#........#..#......#
#..#######..####...#
#..#~~~~~~~~~~#....#
#..#~~~EEEE~~~#....#
#..#~~~E~~E~~~#....#
#..#~~~EEEE~~~#..S.#
#..#~~~~~~~~~~#....#
#..####..######....#
#.....#..#.........#
#..C..#..#....B....#
#.....#..#.........#
#..................#
####################
```

```js
const FLOOR_03_TILES = [
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1,
  1, 0, 0, 1, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 1, 0, 0, 0, 0, 1,
  1, 0, 0, 1, 8, 8, 8, 0, 0, 0, 0, 8, 8, 8, 1, 0, 0, 0, 0, 1,
  1, 0, 0, 1, 8, 8, 8, 0, 8, 8, 0, 8, 8, 8, 1, 0, 0, 0, 0, 1,
  1, 0, 0, 1, 8, 8, 8, 0, 0, 0, 0, 8, 8, 8, 1, 0, 0, 4, 0, 1,
  1, 0, 0, 1, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 1, 0, 0, 0, 0, 1,
  1, 0, 0, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 7, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 6, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
];
```

**Enemy placements**
- `small_slime` at `(7, 5)` — stationary; stationary
- `small_slime` at `(8, 5)` — stationary; stationary
- `sewer_rat` at `(9, 5)` — patrol; patrol (9,5) → (10,5)
- `small_slime` at `(10, 5)` — stationary; stationary
- `small_slime` at `(7, 6)` — stationary; stationary
- `sewer_rat` at `(10, 6)` — patrol; patrol (9,6) → (10,6)
- `small_slime` at `(7, 7)` — stationary; stationary
- `small_slime` at `(8, 7)` — stationary; stationary
- `small_slime` at `(9, 7)` — stationary; stationary
- `sewer_rat` at `(10, 7)` — patrol; patrol (9,7) → (10,7)

**Chest descriptors**
- `f3_ledge` at `(3, 11)` — `band1-chest`, hidden=false, requiresClear=true, hint: boots or coins

**Barrels**
- barrel at `(14, 11)` — `band1-barrel`, guaranteed=false

**Ooh! moment:** The first big chamber opens up and the middle looks like a slimy playground the enemies already claimed.

**Atmosphere note:** More open and splashy than spooky; the room feels like a messy underground gymnasium.

### Floor 4 — The Hidden Cache

**Theme:** Straightforward loops with one breakable secret wall hiding a bonus room.

```text
####################
#P.................#
#..######..######..#
#..#....#..#....#..#
#..#..E.#..#.E..#..#
#..#....#..#....#..#
#..####.#..#.####..#
#...E...#..#...E...#
#.E.....#XX#....E..#
#.......#CC#.......#
#..####.#..#.####..#
#..#....#..#....#..#
#..#..B.#..#..C.#S.#
#..#....#..#....#..#
####################
```

```js
const FLOOR_04_TILES = [
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 1, 12, 12, 1, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 1, 7, 7, 1, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 0, 6, 0, 1, 0, 0, 1, 0, 0, 7, 0, 1, 4, 0, 1,
  1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1,
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
];
```

**Enemy placements**
- `sewer_rat` at `(6, 4)` — patrol; patrol (5,4) → (6,4)
- `small_slime` at `(13, 4)` — stationary; stationary
- `small_slime` at `(4, 7)` — stationary; stationary
- `sewer_rat` at `(14, 7)` — patrol; patrol (13,7) → (15,7)
- `sewer_rat` at `(2, 8)` — patrol; patrol (1,8) → (4,8)
- `small_slime` at `(15, 8)` — stationary; stationary

**Chest descriptors**
- `f4_visible` at `(14, 12)` — `band1-chest`, hidden=false, requiresClear=false, hint: gear roll
- `f4_secret_a` at `(9, 9)` — `band1-hidden`, hidden=true, requiresClear=false, hint: gold bundle
- `f4_secret_b` at `(10, 9)` — `band1-hidden`, hidden=true, requiresClear=false, hint: rare early material

**Barrels**
- barrel at `(6, 12)` — `band1-barrel`, guaranteed=false

**Side room hook**
- `secret-cache` via reveal tile `(9, 8)`; bounds `{'x': 8, 'y': 8, 'w': 4, 'h': 3}`

**Ooh! moment:** Hitting the slightly wrong-looking wall pops open a cheerful cache room packed with chest sparkle.

**Atmosphere note:** This floor feels mischievous rather than dangerous, like the sewer itself is letting the player in on a joke.

### Floor 5 — The Antechamber

**Theme:** A wider pre-checkpoint fight with a gooey centre and enemies arriving from several angles.

```text
####################
#P....####.........#
#.....#..#....E....#
#..E..#..#.........#
#.....#..#####.###.#
#..##############..#
#..#....E....E..#..#
#..#.~~~~~~~~~~.#..#
#..#.~~EEEE~~..#S..#
#..#.~~~~~~~~~~.#..#
#..#....E....B..#..#
#..##############..#
#.......C..........#
#....E........E....#
####################
```

```js
const FLOOR_05_TILES = [
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
  1, 0, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1,
  1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 8, 8, 0, 0, 0, 0, 8, 8, 0, 0, 1, 4, 0, 0, 1,
  1, 0, 0, 1, 0, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 6, 0, 0, 1, 0, 0, 1,
  1, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 7, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1,
  1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
];
```

**Enemy placements**
- `sewer_rat` at `(14, 2)` — patrol; patrol (13,2) → (16,2)
- `small_slime` at `(3, 3)` — stationary; stationary
- `sewer_rat` at `(7, 6)` — patrol; patrol (6,6) → (9,6)
- `small_slime` at `(12, 6)` — stationary; stationary
- `small_slime` at `(7, 8)` — stationary; stationary
- `small_slime` at `(8, 8)` — stationary; stationary
- `sewer_rat` at `(9, 8)` — patrol; patrol (8,8) → (10,8)
- `small_slime` at `(10, 8)` — stationary; stationary
- `small_slime` at `(8, 10)` — stationary; stationary
- `sewer_rat` at `(5, 13)` — patrol; patrol (4,13) → (7,13)
- `sewer_rat` at `(14, 13)` — patrol; patrol (12,13) → (15,13)

**Chest descriptors**
- `f5_reward` at `(8, 12)` — `band1-chest`, hidden=false, requiresClear=true, hint: pre-checkpoint upgrade

**Barrels**
- barrel at `(13, 10)` — `band1-barrel`, guaranteed=false

**Ooh! moment:** The room finally asks the player to kite, circle, and choose targets instead of simply poking forward.

**Atmosphere note:** Big, loud, and splashy — the first floor that feels like a real “deep dive” before a proud checkpoint save.

## 4. Room Template Catalogue (Floors 6–25)

**Template legend:** add `! = steam jet`, `d = drip trap`, `L = lantern`, `G = sewer grate`, `X = secret wall` to the base floor legend above.

### `entry-room`

```text
#########
#...L...#
#.......#
#..P....#
#.......#
#...B...#
#########
```

- **Connection points:** E or S
- **Content tags:** empty, barrel, lantern
- **Placement rules:** Always first. Exactly one player spawn. Never contains elites.
- **Rarity weight:** 5

### `exit-room`

```text
#########
#...L...#
#.......#
#...S...#
#.......#
#.......#
#########
```

- **Connection points:** W or N
- **Content tags:** enemies, stairs, lantern
- **Placement rules:** Always last. Place locked STAIRS_DOWN at centre back wall; convert on clear.
- **Rarity weight:** 5

### `straight-corridor-h`

```text
#########
#.......#
#.G...G.#
#.......#
#..E.E..#
#.......#
#########
```

- **Connection points:** E, W
- **Content tags:** enemies, hazard, empty
- **Placement rules:** Use to stretch pacing between set-piece rooms. Keep 1-tile-wide clear lane.
- **Rarity weight:** 5

### `straight-corridor-v`

```text
#########
#...E...#
#...G...#
#.......#
#...G...#
#...E...#
#########
```

- **Connection points:** N, S
- **Content tags:** enemies, hazard, empty
- **Placement rules:** Vertical equivalent of straight-corridor-h. Prefer after an L-bend.
- **Rarity weight:** 5

### `L-bend-ne`

```text
#########
#.....###
#.....###
#..L..###
#....E..#
#.......#
#########
```

- **Connection points:** N, E
- **Content tags:** enemies, empty, lantern
- **Placement rules:** Corner connector. Rotate only by template variant, not runtime rotation.
- **Rarity weight:** 4

### `L-bend-nw`

```text
#########
###.....#
###.....#
###..L..#
#..E....#
#.......#
#########
```

- **Connection points:** N, W
- **Content tags:** enemies, empty, lantern
- **Placement rules:** Mirror of L-bend-ne. Use to stop hallways from feeling too rectangular.
- **Rarity weight:** 4

### `T-junction`

```text
#########
#...L...#
#.......#
#.E...E.#
#.......#
#...G...#
#########
```

- **Connection points:** N, E, W
- **Content tags:** enemies, branch, lantern
- **Placement rules:** Branch-capable template. Good parent for chest or barrel side paths.
- **Rarity weight:** 4

### `crossroads`

```text
#########
#...G...#
#.......#
#.E.~.E.#
#.......#
#...G...#
#########
```

- **Connection points:** N, S, E, W
- **Content tags:** enemies, branch, hazard
- **Placement rules:** Use sparingly so layout stays readable. Never place adjacent to another crossroads.
- **Rarity weight:** 3

### `small-arena`

```text
#########
#.......#
#.E...E.#
#...L...#
#.E...E.#
#.......#
#########
```

- **Connection points:** Any 1–2 sides
- **Content tags:** enemies, chest, lantern
- **Placement rules:** Requires at least 2 enemies. Great slot for the guaranteed ooh! moment.
- **Rarity weight:** 4

### `large-arena`

```text
#########
#.......#
#.E...E.#
#..~~~..#
#.E...E.#
#...L...#
#########
```

- **Connection points:** Any 1–2 sides
- **Content tags:** enemies, hazard, elite
- **Placement rules:** Max 1 per floor. Eligible for mini-bosses on floors 10 and 20.
- **Rarity weight:** 2

### `chest-alcove`

```text
#########
#####...#
#####.C.#
#####...#
#####...#
#####...#
#########
```

- **Connection points:** One side only
- **Content tags:** chest, empty
- **Placement rules:** Branch endpoint only. Max 1 per floor.
- **Rarity weight:** 2

### `barrel-room`

```text
#########
#.B...B.#
#.......#
#...C...#
#.......#
#.B...B.#
#########
```

- **Connection points:** One side only
- **Content tags:** barrel, chest, empty
- **Placement rules:** Branch endpoint or early breather room. At least 3 barrels.
- **Rarity weight:** 3

### `secret-room`

```text
#########
#..C.C..#
#.......#
#...L...#
#.......#
#..C.C..#
####X####
```

- **Connection points:** Hidden W or hidden E
- **Content tags:** chest, secret, lantern
- **Placement rules:** Max 1 per floor. Must be behind SECRET_WALL and off the main path.
- **Rarity weight:** 1

### `hazard-corridor`

```text
#########
#..!~!..#
#.......#
#.d.E.d.#
#.......#
#..!~!..#
#########
```

- **Connection points:** E, W or N, S
- **Content tags:** hazard, enemies
- **Placement rules:** Contains at least one hazard tile. Keep a safe rhythm lane for readable dodging.
- **Rarity weight:** 3

### `boss-antechamber`

```text
#########
#...L...#
#..G.G..#
#.......#
#..S.S..#
#.......#
#########
```

- **Connection points:** W only
- **Content tags:** empty, lantern, lore
- **Placement rules:** Floor 25 only. No regular enemies. Leads directly to boss door / conversation gate.
- **Rarity weight:** 1

## 5. Procedural Stitching Rules (Floors 6–25)

**Approach:** graph-based room stitching, not noise. A floor is a small room tree whose main path goes from `entry-room` to `exit-room`, then rasterises into a flat tile array.

### 5.1 Layout rules
1. Always start with `entry-room`, always end with `exit-room`.
2. Main path length = **3–6 rooms**; total placed rooms usually land at **4–7** once branches are counted.
3. Side branches = **0–2**; each branch is **1–2 rooms** and ends in `chest-alcove`, `barrel-room`, or `secret-room`.
4. No loops in v0.5/v1.0: the room graph is a **tree**.
5. Room templates sit on a coarse grid with **1-tile doorway links** between touching room edges.
6. `chest-alcove`, `secret-room`, and `large-arena` are each **max 1 per floor**.
7. Every floor gets **one guaranteed “ooh!” moment** chosen from the weighted table in §6.
8. Enemy count and loot table come from the current floor band, then distribute into room tags (`small-arena`, `hazard-corridor`, `crossroads`, etc.).
9. Floor 25 skips normal generation and uses `entry-room` → `boss-antechamber` → boss gate logic.

### 5.2 Deterministic seeding
Use a floor-local seed that depends only on zone + floor number:

```js
function sewerSeed(floorNumber) {
  return hash32(`moonwater-sewer-${floorNumber}`);
}
```

A replayed floor number should regenerate identically so players can learn routes, secrets, and favourite chest branches.

### 5.3 Pseudocode
```js
function buildSewerFloor(floorNumber) {
  const band = getBand(floorNumber);
  const rng = mulberry32(sewerSeed(floorNumber));

  const graph = new RoomTree();
  graph.add('entry-room');

  const mainPathLength = randInt(rng, 3, 6);
  for (let i = 1; i < mainPathLength - 1; i++) {
    graph.addToMainPath(pickMiddleTemplate(rng, band, graph));
  }
  graph.addToMainPath('exit-room');

  const branchCount = randInt(rng, 0, 2);
  for (let i = 0; i < branchCount; i++) {
    const branchRoot = pickBranchableRoom(rng, graph); // T-junction or crossroads preferred
    const branchLength = randInt(rng, 1, 2);
    graph.addBranch(branchRoot, buildBranch(rng, branchLength));
  }

  graph.enforceCaps({
    'large-arena': 1,
    'chest-alcove': 1,
    'secret-room': 1,
  });

  placeRoomsOnCoarseGrid(graph, { doorwayWidth: 1, gap: 0 });
  const floor = rasterizeGraphToTileArray(graph);

  injectGuaranteedMoment(floor, rng, band);
  populateEnemies(floor, rng, getEncounterBudget(floorNumber));
  populateLootables(floor, rng, band);
  lockStairsUntilClear(floor);

  return floor;
}
```

## 6. Encounter Tables

| Band | Floors | Enemy mix | Enemy count | Elite / mini-boss logic | Loot table | Guaranteed ooh! moment table |
| --- | --- | --- | --- | --- | --- | --- |
| Band 1 | Floors 1–5 | Small Slime 55%, Sewer Rat 45% | 6–10 | No elites. Hand-designed tutorial floors. | Gold (small), Slime Glob 30%, Rat Pelt 20% | Extra chest 60%, bonus gold barrel 30%, rare item drop 10% |
| Band 2 | Floors 6–10 | Small Slime 50%, Sewer Rat 50% | 10–14 | 10% elite room rate; Floor 10 forces The Pipe Warden. | Gold (medium), Slime Glob 35%, Rat Pelt 25%, Cracked Pipe 10% | Extra chest 40%, mini-boss 30%, secret room 20%, lore scrawl 10% |
| Band 3 | Floors 11–15 | Small Slime 30%, Sewer Rat 30%, Toxic Slime 40% | 12–15 | 18% elite room rate; 0–1 named encounter. | Gold (medium), Toxic Goop 35%, Rat Pelt 20%, Glowing Fungus 15% | Chest 35%, mini-boss 35%, secret room 20%, lore scrawl 10% |
| Band 4 | Floors 16–20 | Small Slime 20%, Toxic Slime 40%, Pipe Spider 40% | 13–16 | 22% elite room rate; Floor 20 forces The Slime Mother. | Gold (large), Spider Silk 30%, Toxic Goop 25%, Glowing Fungus 20% | Chest 30%, mini-boss 40%, secret room 20%, rare cosmetic 10% |
| Band 5 | Floors 21–24 | Toxic Slime 30%, Pipe Spider 30%, Giant Slime 40% | 14–18 | 28% elite room rate; giant slime packs replace weak trash. | Gold (large), Giant Slime Core 20%, plus all Band 4 drops | Chest 25%, mini-boss 45%, secret room 20%, rare cosmetic 10% |
| Boss Gate | Floor 25 | No regular enemies | 0 | No elite roll; boss door conversation gate only. | None until boss engagement | Silent lead-in, breathing walls, moonwater residue |

### 6.1 Placement notes
- Put faster enemies (Rats, Spiders) in **corridor** and **junction** templates.
- Put bulkier enemies (Toxic Slimes, Giant Slimes) in **small-arena** or **large-arena** templates so there is room to read them.
- Side branches should feel like a reward, not a tax: at least one branch node per generated floor should be short and visually legible.
- Floor 25 is intentionally quiet. No random trash mobs, no loot spam, just a breath before the Slime King gate.

## 7. Enemy Design Reference

### Small Slime

- **Sprite description:** A squishy teardrop blob with two bright eyes, a jelly wobble outline, and a tiny smile when idle.
- **Behaviour:** Slow hop patrol. Notices the player at 4 tiles, then short-hops forward and body-checks. In slime puddles it gets a small speed boost so the puddle feels “its home turf.”
- **Stats:** HP 18→34 (floors 1–10) · Damage 4→10 · Move 1.4 tiles/s · Attack every 1.6 s · XP 5→15 · Gold 2–4
- **Death effect:** Squishes flat, splits into two tiny harmless droplets for 0.4 s, then pops into sparkly goo.
- **Sound cues:** `sfx_slime_squish`, `sfx_slime_pop`

### Sewer Rat

- **Sprite description:** Round-bodied rat with shiny bead eyes, oversized ears, and a waggle tail drawn with quick brush strokes.
- **Behaviour:** Fast lane-runner. Patrols straight corridors, aggroes at 5 tiles, darts in for a nibble, then sidesteps. Likes flanking more than tanking.
- **Stats:** HP 14→30 (floors 1–15) · Damage 5→12 · Move 2.0 tiles/s · Attack every 1.2 s · XP 6→24 · Gold 2–5
- **Death effect:** Flips onto its back with little cartoon stars, then scoots off-screen as if embarrassed.
- **Sound cues:** `sfx_rat_squeak`, `sfx_rat_scurry`

### Toxic Slime

- **Sprite description:** A darker slime with neon-green pockets, drifting bubbles, and a faint toxic rim-light.
- **Behaviour:** Lobs globs at 3–4 tiles and leaves mini puddles on impact. Chases more slowly than a rat but controls space.
- **Stats:** HP 36→68 (floors 10–20) · Damage 10→19 · Move 1.3 tiles/s · Attack every 1.7 s · XP 22→42 · Gold 4–8
- **Death effect:** Bursts in a ring of harmless bubbles and leaves a fading goop decal for 1 second.
- **Sound cues:** `sfx_toxic_bloop`, `sfx_toxic_spit`

### Pipe Spider

- **Sprite description:** A squat pipe-dwelling spider with copper legs, lantern-reflective eyes, and a damp pipe-shell back.
- **Behaviour:** Waits on walls or ceilings, drops when the player enters 3 tiles, then skitters and fires a short web spit that briefly slows.
- **Stats:** HP 30→62 (floors 15–25) · Damage 11→21 · Move 1.8 tiles/s · Attack every 1.4 s · XP 32→52 · Gold 5–9
- **Death effect:** Legs curl inward and the body unspools into a tidy silk knot pickup.
- **Sound cues:** `sfx_spider_click`, `sfx_web_flick`

### Giant Slime

- **Sprite description:** A room-filling cheerful pudding of slime with suspended junk inside: bottle caps, a spoon, maybe a lost boot.
- **Behaviour:** Heavy bounce, short charge, and splash ring on landing. Takes space rather than chasing efficiently.
- **Stats:** HP 120→240 (floors 20–24) · Damage 18→30 · Move 0.9 tiles/s · Attack every 2.1 s · XP 45→56 · Gold 12–20
- **Death effect:** Collapses into a giant wobble pancake, jiggles twice, then sucks inward with a comic slurp.
- **Sound cues:** `sfx_giant_slime_whomp`, `sfx_giant_slime_slurp`

## 8. Mini-Boss Design (floors 10 and 20)

### Floor 10 mini-boss — **The Pipe Warden**
A named elite Sewer Rat wearing bolted pipe armour and dragging a valve-wheel collar like a badge of office.

- **Spawn room:** `large-arena`
- **HP:** **850**
- **Contact damage:** **16**
- **Move speed:** **1.9 tiles/s**
- **XP:** **220**
- **Gold:** **90–120**
- **Phase 1 (100%–50%)** — fast lane charges, triple-bite combo, and a tail-slap cone. Every 8 seconds it opens a side pipe and floods a lane with harmless-but-slowing slime.
- **Phase 2 (50%–0%)** — armour cracks off, it gets angrier and starts ricocheting between arena walls. After every second ricochet it summons **2 Sewer Rats**.
- **Special ability:** **Valve Burst** — a 1-second whistle telegraph, then four cardinal steam jets fire at once.
- **Loot:** guaranteed Tier-2 chest roll, `Pipewarden Plate` unlock chance, `Cracked Pipe Bundle ×3`, medium gold pile.
- **Dialogue unlock:** Rex gets a new tavern line about “the one rat even the old crews left alone,” and Shade hints that something larger is organising the moonwater below.

### Floor 20 mini-boss — **The Slime Mother**
An enormous Toxic Slime with little bright eyes floating inside it — the first clear sign that the Slime King may have a whole happy family.

- **Spawn room:** `large-arena`
- **HP:** **2,200**
- **Contact damage:** **24**
- **Move speed:** **1.1 tiles/s**
- **XP:** **420**
- **Gold:** **180–240**
- **Phase 1 (100%–60%)** — slow leaps, toxic lob shots, and a big puddle bloom under landing points.
- **Phase 2 (60%–0%)** — every 12 seconds it spawns **2 Small Slimes** or **1 Toxic Slime**, then does a belly-flop shockwave that rewards diagonal dodging.
- **Special ability:** **Nursery Call** — all active puddles briefly pulse, then release tiny harmless bubbles as a warning before new adds appear.
- **Loot:** guaranteed Tier-3 chest roll, `Toxic Goop Bundle ×3`, `Glowing Fungus ×2`, high gold pile, small chance at a cosmetic slime bow.
- **Dialogue unlock:** Shade softens his tone and notes that the “king” below might be less a tyrant and more a very messy guardian; one child-friendly clue toward the warm twist of Floor 25.

---

*Dungeon layouts should feel damp, playful, and memorable: just enough tension to feel brave, never enough to feel unwelcome.*
