# 🚧 Moonlight City — Development Stages

> Companion to [ROADMAP.md](./ROADMAP.md) and [TODO.md](./TODO.md).  
> Use this as the **practical build order**: small stages, fun checkpoints, always-playable endings.

---

## 🌟 How to use this plan

- Treat each stage like a mini season of the project.
- Finish the **acceptance criteria** before moving on.
- If a stage grows too large, **cut content, not stability**.
- `TODO.md` remains the backlog. `ROADMAP.md` remains the big-picture promise.  
  **This file is the hands-on path from prototype to v0.5 Demo Day.**

Target rhythm from [ROADMAP.md §4](./ROADMAP.md): short shared sessions, visible wins, and a playable build after every coding day.

---

## Stage 0 — "The Walking World" [tag: v0.0-stage0]

**Goal:** Lock in the movement sandbox and treat it as the stable foundation.
**Playable at end:** You can open `game.html`, walk a character around a little world, bump into trees/rocks/water, and watch the camera and simple animation work.
**Estimated sessions:** Done

### Tasks
- [x] Create the single-file HTML/Canvas prototype — *the project boots instantly in the browser*
- [x] Build a 40×40 hand-made tile world — *grass, dark grass, trees, water, paths, rocks, flowers*
- [x] Add smooth movement and collision — *acceleration, deceleration, circle-vs-tile blocking*
- [x] Add camera follow — *keep the player near screen centre with lerp*
- [x] Add simple world animation — *water shimmer and tree sway*
- [x] Add a day counter UI — *prototype timing only; final city clock changes in later stages*

### Acceptance criteria
- The game runs from one file with no build step
- The player can move with WASD or arrow keys
- Blocking tiles stop movement cleanly
- The camera follows without jitter
- The prototype is stable enough to keep extending instead of restarting

### Session pickup note
> This is the solid ground under the whole project. Nothing fancy needs changing here first — just keep it runnable, readable, and pleasant to move around in. Every later stage should feel like “adding a new toy to the playground,” not rebuilding the playground.

---

## Stage 1 — "A Face in the Crowd" [tag: v0.1-stage1]

**Goal:** Let the player make a simple version of themselves, then step into the city wearing their own choices.
**Playable at end:** From the title screen, the player can start a new game, choose a name/look, save it, and walk through a bigger three-district city as that character.
**Estimated sessions:** 4

### Tasks
- [ ] Add a title screen with Start and Continue — *Continue only lights up when a valid save exists*
- [ ] Build a canvas-based character creation scene — *no HTML forms; all UI is drawn in the game*
- [ ] Add name entry — *simple text input with a short max length*
- [ ] Add 4 skin tones — *index into a palette table*
- [ ] Add 6 hair styles — *small procedural sprite variations, not separate assets*
- [ ] Add 8 hair colours — *palette swap only*
- [ ] Add 3 outfits — *recolour torso/legs with maybe one shape tweak*
- [ ] Lock v0.5 to 1 class — *store the chosen class even if there is only one option for now*
- [ ] Save character data to localStorage — *versioned save object from day one*
- [ ] Load save data on Continue — *skip character creation when a save exists*
- [ ] Expand the city to 64×64 tiles — *keep it hand-authored and readable*
- [ ] Split the city visually into Downtown, Residential, and Slums — *use paths, palette, building outlines, fences, clutter*
- [ ] Add the player's house exterior marker — *a clear future entrance point*

### Acceptance criteria
- Title screen appears before gameplay
- New Game always reaches character creation
- Continue loads the saved character correctly
- Chosen name/look persists after refresh
- The player sprite visibly changes based on skin, hair, hair colour, and outfit choices
- The city feels like three distinct districts, even with placeholder buildings
- `v0.1` can be shown to someone and they immediately understand: “I made this character, and now I’m in this town”

### Session pickup note
> This stage is about identity. You are not building “menus”; you are building the moment where the player says, “That’s me.” If a choice does not clearly show up on the character or in the city, it can wait.

---

## Stage 2 — "Down in the Dark" [tag: v0.2-stage2]

**Goal:** Make the sewer dungeon real enough to enter, fight through, and lose in.
**Playable at end:** The player can find the sewer entrance, descend into three floors, attack slimes and rats, clear rooms, climb stairs, and wake back in the city after death.
**Estimated sessions:** 6

### Tasks
- [ ] Place the sewer entrance trigger in the Slums — *make it visually obvious and memorable*
- [ ] Add a fade transition system — *reusable for city, house, and death transitions*
- [ ] Create the dungeon scene shell — *separate map, enemy list, drops list, floor label*
- [ ] Add a dungeon tileset style — *dark stone, pipes, slime, puddles, lantern glow*
- [ ] Build 3 hand-designed floor layouts — *store as data arrays, not hard-coded draw calls*
- [ ] Add player HP and MP bars — *top-left HUD is fine*
- [ ] Implement the basic attack — *button press spawns a short melee hitbox in facing direction*
- [ ] Add damage numbers — *simple pop-up text with upward drift*
- [ ] Create Small Slime — *wander, chase, short melee bump attack*
- [ ] Create Sewer Rat — *lower HP, faster movement, quick bite*
- [ ] Add enemy HP bars — *only when hurt or nearby, to reduce clutter*
- [ ] Add enemy death and gold drop — *coin pop + pickup sound placeholder later*
- [ ] Add floor-clear logic — *stairs stay locked until all enemies are gone*
- [ ] Add stairs to next floor — *spawn or unlock after clear*
- [ ] Add death handling — *fade out, lose 20% carried gold, respawn at fixed city safe point*
- [ ] Save deepest cleared floor and checkpoint floor — *store in save schema even if checkpoint stays simple for now*

### Acceptance criteria
- The sewer entrance reliably transitions from city to dungeon
- Three unique floors can be played start to finish
- Both enemy types move, attack, take damage, and die
- The player can attack and dash without breaking movement
- Gold drops can be picked up
- Stairs only open after all enemies are defeated
- Death returns the player to the city and removes only carried gold, not items
- Deepest floor progress survives page refresh

### Session pickup note
> This is the first “real game” feeling stage. Keep asking one question: does going down feel exciting? Fancy combat systems can wait. A slime that is fun to bonk is worth more right now than ten future mechanics on paper.

---

## Stage 3 — "Pockets Full" [tag: v0.3-stage3]

**Goal:** Make loot matter so dungeon runs feed the city loop.
**Playable at end:** The player can earn gold and junk, open an inventory, buy and sell at Mara’s shop, use potions, and bank money back home.
**Estimated sessions:** 5

### Tasks
- [ ] Add gold to the HUD — *show carried gold clearly at all times*
- [ ] Build the inventory overlay — *canvas grid, item icons, qty labels, selected item panel*
- [ ] Add junk drops — *start with Slime Glob and Rat Pelt*
- [ ] Add random gold pickup values — *small floor-scaled amounts feel better than a flat 1 coin*
- [ ] Add barrel breakables — *attack or click to smash; 50% chance to drop loot*
- [ ] Put Mara in the city at a fixed shop spot — *her dialogue can stay minimal until Stage 5*
- [ ] Build Mara’s shop mode inside the inventory overlay — *same UI shell, different stock list*
- [ ] Add 3 potions — *HP restore now; extra effects can remain future-proofed*
- [ ] Add 5 furniture items for sale — *these are mostly preparing Stage 4*
- [ ] Add sell-junk flow — *select junk item, receive gold*
- [ ] Add item use from inventory — *consume potion, heal player, reduce quantity*
- [ ] Save carried and banked gold separately — *match the soft-death loop from ROADMAP.md*
- [ ] Add a home chest or bank point in the starter shack flow — *deposit carried gold into safe storage*

### Acceptance criteria
- Gold is earned in the dungeon and shown in the HUD
- Inventory opens and closes cleanly from gameplay
- At least two junk items drop and stack properly
- Barrels can be broken and sometimes reward loot
- Mara’s shop can buy and sell items without corrupting inventory
- Potions can be used and affect HP
- Banked gold is saved separately from carried gold
- After one dungeon run, the player has a clear reason to come back to the city

### Session pickup note
> This stage is about reward. The sewer should no longer feel like a place you visit “because that’s the level.” It should feel like a place you go because treasure comes back with you. If the player grins when the gold number jumps up, you’re on the right track.

---

## Stage 4 — "A Room of One's Own" [tag: v0.4-stage4]

**Goal:** Turn the house into a place where loot lands and personality shows up.
**Playable at end:** The player can enter their house, choose wall/floor colours, place owned furniture on a grid, remove it, save the room, and come back to find it unchanged.
**Estimated sessions:** 6

### Tasks
- [ ] Add the house interior scene — *one room, one doorway, clear cosy palette*
- [ ] Add house entrance trigger from the city — *walk into the house marker to enter*
- [ ] Draw a visible placement grid — *subtle lines, only strong when placing furniture*
- [ ] Add furniture ownership data — *shop purchases should mark items as owned/placeable*
- [ ] Implement furniture selection mode — *choose from owned furniture list, then place in room*
- [ ] Implement grid-snap placement — *mouse highlights a valid or invalid footprint*
- [ ] Add 8 starter furniture sprites — *bed, table, chair, chest, lamp, rug, bookshelf, plant*
- [ ] Add wall colour swatches — *6 fixed choices*
- [ ] Add floor colour swatches — *6 fixed choices*
- [ ] Add furniture removal — *click selected placed item to remove back into owned pool*
- [ ] Save house layout to localStorage — *placed items, wall colour, floor colour*
- [ ] Load house layout on boot and on re-entry — *the room should feel persistent and personal*
- [ ] Add exit door back to city — *same simple transition pattern as dungeon*

### Acceptance criteria
- The player can enter and leave the house from the city
- Furniture placement snaps to the room grid instead of floating freely
- Invalid placements are blocked cleanly
- All 8 starter furniture items can be placed and removed
- Wall and floor colours visibly change
- House layout persists after refresh
- The house feels like a reward space, not just another menu

### Session pickup note
> This is the cosy heart of the whole loop. Keep the first room small and easy. A tiny room you can truly own is better than a giant editor that feels unfinished. The test is simple: after a run, does the player want to go home and fiddle with the room for a minute?

---

## Stage 5 — "Warm Lights, New Friends" [tag: v0.5-stage5]

**Goal:** Close the full demo loop with NPCs, dialogue, sound, title polish, and game-feel.
**Playable at end:** The player can start at a polished title screen, talk to Mara/Rex/Lumi, build small relationships, fight and shop with sound and feedback, pause the game, and enjoy a complete 10-minute demo-day play session.
**Estimated sessions:** 5

### Tasks
- [ ] Add 3 NPC sprites in fixed city positions — *Mara, Rex, Lumi*
- [ ] Add click-to-talk interaction — *bubble prompt or hover highlight helps discoverability*
- [ ] Build the dialogue overlay — *greeting line, 2–3 topic buttons, close action*
- [ ] Write flat v0.5 topic sets — *short, warm, flavourful; save branching for later*
- [ ] Add relationship tracking with 3 visible levels — *different greeting line at each level*
- [ ] Save relationship state per NPC — *simple `{npcId: level}` map in save data*
- [ ] Connect Mara’s dialogue to shop opening — *talking to her should lead naturally into buying*
- [ ] Add 6–8 sound effects — *footstep, swing, hit, coin, door, UI click, buy, potion*
- [ ] Add 1 city music loop — *start after first user input to satisfy browser audio rules*
- [ ] Polish the title screen — *logo, animated background, Start/Continue/Credits*
- [ ] Add the pause menu — *Resume, Save & Title, Quit to Title*
- [ ] Add the game over screen/overlay — *gentle fail state with clear return path*
- [ ] Add hit-stop on solid attacks — *very short freeze for punchy impact*
- [ ] Add screen shake on heavy hits — *subtle, not headache-inducing*
- [ ] Improve damage number animation — *rise, fade, maybe crit colour*
- [ ] Do a playtest polish pass — *fix friction before adding anything new*

### Acceptance criteria
- Mara, Rex, and Lumi are visible and clickable in the city
- Dialogue opens as an overlay and closes cleanly
- Relationship level changes at least one greeting per NPC
- Relationship values persist after reload
- Shop, inventory, house, and dungeon loops all still work together
- Sound effects and one music loop play reliably
- Pause works from gameplay scenes
- A new player can complete a short loop: start → city → dungeon → loot → shop/house → talk to NPC
- The build is confident enough for **Demo Day**

### Session pickup note
> This stage is about warmth. The systems already exist; now they need personality, sound, and “juice.” Small touches matter a lot here: a nice click sound, a kind greeting from Mara, a tiny screen shake on a good hit. This is where the prototype starts feeling loved.

---

## 🎉 After the Stages — What v0.5 Demo Day Looks Like

A good Demo Day playtest lasts about **10 minutes**:

1. The player sees a welcoming title screen
2. They start or continue a save
3. They walk through a readable city with three districts
4. They talk to Mara, Rex, or Lumi and feel the city has people in it
5. They enter the sewer and clear a floor or two
6. They come back with gold and loot
7. They buy a potion or furniture item
8. They step into the house and place something
9. They leave saying, “I want one more run”

That is the v0.5 promise from [ROADMAP.md §1](./ROADMAP.md): **Home → City → Dungeon → Haul → Show Off** in a form a friend can understand without explanation.

---

## 🌠 Bridge to v1.0

Once v0.5 is stable, v1.0 mostly adds **depth, not new categories**:

- more dungeon floors
- more enemies and a boss
- more dialogue and stronger relationship payoffs
- more furniture and house upgrades
- more combat abilities and progression
- more city life around the same core loop

In other words: do not throw this plan away after Demo Day.  
Use it as the ladder. v1.0 is mostly **more rungs on the same ladder**.

For combat tuning, XP curves, and boss targets, see [BALANCE.md](./BALANCE.md).  
For narrative flavour and NPC voices, see [STORY.md](./STORY.md).
