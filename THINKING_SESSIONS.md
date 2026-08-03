# 🧠 Moonlight City — Thinking Sessions

> Long-running design & architecture sessions that produce reference documents for the project.
> Each session is a focused deep-dive that would take too long to do inline — they run as background tasks and output one or more new documents.

---

## Session Registry

| ID | Title | Status | Outputs |
|----|-------|--------|---------|
| [A](#session-a) | Architecture & Staged Dev Doc | ✅ Complete | `ARCHITECTURE.md`, `DEVELOPMENT_STAGES.md` |
| [B](#session-b) | Combat & Dungeon Design Tightening | ⬜ Planned | `DUNGEONS.md`, updates to `BALANCE.md` |
| [C](#session-c) | NPC Dialogue & Quest System Spec | ⬜ Planned | `DIALOGUE_SPEC.md`, updates to `STORY.md` |

---

## Session A

**Title:** Architecture & Staged Dev Doc
**Status:** ✅ Complete
**Priority:** ⭐ Most urgent — must run before significant new code is written

### Problem it solves
`game.html` is currently a single 720-line IIFE with no internal structure. The design docs are rich and consistent but contain no technical blueprint — no module layout, no save schema, no scene state machine. Without this, the file will grow into an unmaintainable wall of code.

### Scope
- **JavaScript module/section layout** inside the single HTML file — what sections exist, what each owns, line budgets, dependency order
- **Scene state machine** — TITLE → CHARACTER_CREATION → CITY → HOUSE_INTERIOR → DUNGEON → overlays (DIALOGUE, INVENTORY, PAUSE)
- **Save data schema** — versioned localStorage object covering player, gold, inventory, dungeon progress, house layout, NPC relationships, time, flags; plus a `migrate()` pattern
- **Entity/object model** — plain object shapes for Player, Enemy, NPC, Item, Furniture, DungeonFloor
- **Rendering pipeline** — draw order, Y-sort layering, UI overlay layers
- **Input system** — keyboard map for v0.5, mouse/click routing by active scene
- **Audio system** — SFX list, music loop management
- **Key algorithms** — document what's already there (Y-sort, circle-AABB collision) and what's needed next

### Outputs
- `ARCHITECTURE.md` — technical reference document
- `DEVELOPMENT_STAGES.md` — 5-stage always-playable build plan from today → v0.5 Demo Day, written so a child can follow it

---

## Session B

**Title:** Combat & Dungeon Design Tightening
**Status:** ⬜ Planned
**Depends on:** Session A (architecture must be settled first)

### Problem it solves
The dungeon is designed at two disconnected levels of abstraction:
- **BALANCE.md** has precise numbers (XP/floor, enemy HP, damage formulas)
- **DESIGN.md** has the emotional design (the "guaranteed ooh! moment" per floor, atmosphere)

Neither doc gives a developer the **room-level grammar** or **encounter pacing rules** needed to actually build Sewer floors 1–5.

### Scope
- **Room template catalogue** for Zone 1 (Sewers): entrance room, straight corridor, L-bend, arena room, chest alcove, side secret room, boss antechamber — each with a tile layout sketch and placement rules
- **Enemy encounter rules per floor band** (floors 1–5, 6–10, etc.) — enemy count, type mix, patrol vs. chase radius
- **Procedural stitching spec** — how the 15 hand-designed room templates are stitched for floors 6–25 while preserving the hand-designed feel of floors 1–5
- **The "ooh! moment" contract** — one guaranteed interesting event per floor formalised as a weighted random table
- **Floor hazard rules** — slime puddles (slow), drip traps, pipe steam jets, darkness zones

### Outputs
- `DUNGEONS.md` — new document with room templates, encounter tables, stitching algorithm, hazard rules
- Updates to `BALANCE.md` — floor-by-floor encounter tables linked to the room catalogue

---

## Session C

**Title:** NPC Dialogue & Quest System Spec
**Status:** ⬜ Planned
**Depends on:** Session A (save schema must be settled before quest tracking is designed)

### Problem it solves
`STORY.md` has rich backstories, personality notes, and sample dialogue for all NPCs — but none of it is in a format a developer can implement. There's no data schema for dialogue trees, no spec for how quest state is tracked, and no definition of what "relationship level 2" means in practice.

### Scope
- **Dialogue tree JSON schema** — the data format for all NPC conversations (nodes, choices, conditions, effects)
- **Relationship state machine** — what actions increment/decrement relationship, what each of the 4 tiers (Stranger → Best Friend) unlocks, how it's stored
- **Quest board system** — how "fetch me 5 slime globs" requests are generated, tracked, and completed; the data format for a quest object
- **The 10-minute errand pattern** — the repeatable template `{giver, wantItem, wantQty, goldReward, friendshipReward, flavourText}` as a reusable quest type
- **Time-of-day dialogue variants** — how the same NPC serves different lines at morning/evening/night without exploding the tree size
- **v0.5 dialogue content** — the actual flat dialogue trees for Mara, Rex, and Lumi at all 3 relationship levels, ready to paste into code

### Outputs
- `DIALOGUE_SPEC.md` — new document with schema, state machine, quest format
- Updates to `STORY.md` — Mara/Rex/Lumi dialogue formatted in the new schema

---

## Running a Session

Sessions are launched as background deep-thinking agents. To run a session:

1. Check the registry above — confirm dependencies are met
2. Ask Copilot CLI: *"run thinking session B"* (or C)
3. The agent will produce the output documents and update this registry

---

*Last updated: August 2026*
