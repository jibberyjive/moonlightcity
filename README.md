# 🌙 Moonlight City

A Stardew Valley-inspired 2D game built by a dad and daughter team. 🌱

---

## 🗺️ High-Level Game Development Guide

This guide walks through the major phases of building a 2D life-sim / farming game from scratch. Take it one step at a time — the journey is the fun part!

---

## Phase 1: Pick Your Tools 🛠️

Choose a game engine that's beginner-friendly and good for 2D games. Great options:

| Engine | Language | Why it's great |
|--------|----------|----------------|
| [Godot](https://godotengine.org/) | GDScript (like Python) | Free, lightweight, perfect for 2D |
| [Unity](https://unity.com/) | C# | Huge community, lots of tutorials |
| [Pygame](https://www.pygame.org/) | Python | Great if you already know Python |

**Recommendation for a parent/child team: Godot.** It's free, has a visual editor, and GDScript is easy to learn.

---

## Phase 2: Plan Your Game 📝

Before writing any code, sketch out your ideas together:

- **Setting** — What's the world like? A cozy town? A magical farm? A city?
- **Player character** — Who are you playing as? What do they look like?
- **Core loop** — What does the player *do* every day? (e.g. farm, fish, talk to villagers)
- **Goals** — What is the player working toward? (e.g. restore the town, make friends, build a farm)
- **Villagers** — Who lives in the world? Give them names and personalities!

> 💡 **Fun activity:** Draw your map and characters on paper first. This is a great thing to do together!

---

## Phase 3: Build the Foundation 🧱

Get the basics working before adding features:

1. **Create a simple tile map** — a grid of ground/grass tiles the player can walk on
2. **Add a player character** — a sprite that moves with arrow keys or WASD
3. **Add a camera** — so the camera follows the player around the world
4. **Add collision** — so the player can't walk through walls and trees

---

## Phase 4: Core Gameplay Systems 🌾

Start layering in the features that make it feel like a life-sim:

1. **Day/night cycle** — time passes, screen gets darker at night
2. **Energy system** — player has limited energy each day
3. **Inventory** — player can pick up and hold items
4. **Farming** — till soil, plant seeds, water crops, harvest
5. **Seasons** — crops grow differently in spring, summer, fall, winter

---

## Phase 5: World & Characters 🏘️

Make the world feel alive:

1. **Build the town** — add buildings, paths, decorations
2. **Add NPCs (villagers)** — characters that walk around and have schedules
3. **Dialogue system** — player can talk to villagers
4. **Friendship system** — give gifts, build relationships
5. **Shops** — buy seeds, tools, and sell your crops

---

## Phase 6: Audio & Art 🎨🎵

Polish makes a huge difference:

1. **Pixel art sprites** — player, NPCs, crops, tiles, items
2. **Tileset** — a set of ground/floor/wall tiles for your maps
3. **Sound effects** — footsteps, chopping, watering, UI clicks
4. **Music** — calm background music for the farm, town, and night

> 💡 **Free resources:** [itch.io](https://itch.io/game-assets/free) has tons of free pixel art assets. [OpenGameArt.org](https://opengameart.org/) has free music and sound effects.

---

## Phase 7: Save & Load 💾

Let the player save their progress:

1. Save the game state (day, inventory, farm layout, friendships)
2. Load it back when the game starts

---

## Phase 8: Polish & Share 🚀

Make it feel finished:

1. **Main menu** — title screen with "New Game" and "Continue"
2. **UI** — health/energy bar, calendar, inventory screen
3. **Bug fixing** — play through and fix anything broken
4. **Share it!** — Export the game and share with family and friends

---

## 🌟 Suggested First Weekend Project

To get started quickly, try building just this:

- [ ] A grassy tile map (10x10 is fine!)
- [ ] A player character that moves around
- [ ] One tree the player can't walk through

That's a real game world — everything else builds on top of it!

---

## 📚 Helpful Resources

- [Godot Docs](https://docs.godotengine.org/en/stable/) — official documentation
- [GDQuest Godot tutorials](https://www.gdquest.com/) — excellent free tutorials
- [Brackeys on YouTube](https://www.youtube.com/@Brackeys) — beginner-friendly game dev videos
- [Stardew Valley wiki](https://stardewvalleywiki.com/) — great for inspiration on game systems

---

*Happy farming! 🌻*
