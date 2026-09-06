# Moonlight City - Graphics Direction Proposals

## Goal

Give Moonlight City a major visual upgrade with the warmth, readability, expressive characters, and playful presentation associated with modern life-simulation games.

The new direction should feel inspired by that genre without copying The Sims 4's protected characters, assets, icons, interface, animations, branding, or exact visual design. Moonlight City should remain visually recognizable as its own game.

## Current visual baseline

Moonlight City currently uses:

- A full-screen HTML canvas
- Top-down tile maps
- Small hand-drawn pixel characters
- Flat-colour buildings and interiors
- UI rendered directly through the canvas
- No external art pipeline or build process

A large improvement can either preserve this technical structure or deliberately replace parts of it. The three proposals below represent different levels of ambition.

---

## Direction 1 - Moonlit Dollhouse

**Recommended**

Turn the game into a colourful 2.5D dollhouse world. Rooms use an angled cutaway view, characters are larger paper-doll figures, and environments use layered lighting and shadows. This is the closest option to the friendly household fantasy of The Sims 4 while remaining original.

### Visual identity

- Soft 2.5D rooms shown from a raised three-quarter angle
- Larger characters with expressive faces and readable outfits
- Rounded furniture with clean silhouettes and gentle gradients
- Bright daytime colours and rich blue-purple moonlit scenes
- Character selection shown with a floating crystal-moon marker
- Smooth camera easing, object highlights, and short reaction animations
- Spacious card-based UI with rounded panels and large readable icons

### Character direction

- Characters become approximately three times larger on screen
- Separate head, hair, torso, legs, and accessory layers
- More face shapes, eyes, eyebrows, mouths, and skin tones
- Idle poses and emotional reactions based on mood
- Outfit silhouettes remain readable at normal gameplay distance
- Create Your Life uses a full-height animated preview

### Environment direction

- House rooms become cutaway sets with visible wall depth
- City buildings gain roofs, awnings, windows, signs, and cast shadows
- Trees and street furniture use layered shapes instead of single flat tiles
- Interiors receive foreground objects to create depth
- Weather adds ground reflections, moving shadows, and window effects

### UI direction

- Bottom portrait dock for household members
- Needs displayed as compact icon bars
- Action queue displayed beside the active character
- Context actions appear near the selected object
- Consistent teal, cream, coral, and moon-blue palette

### Technical approach

Keep the existing HTML canvas and JavaScript architecture. Add a reusable layered-sprite renderer, depth sorting, room projection helpers, animation states, and a unified UI component library.

### Scope and risk

| Measure | Estimate |
|---|---|
| Visual impact | Very high |
| Engineering effort | High |
| New art required | High |
| Risk to existing systems | Medium |
| Best for | A distinctive long-term visual identity |

---

## Direction 2 - Deluxe Pixel Life

Preserve the current top-down pixel-art structure but rebuild its presentation at a much higher level of polish. This is the safest route and allows improvements to ship district by district.

### Visual identity

- High-detail 32-bit pixel art with a warm illustrated palette
- Larger tiles and richer environmental texture
- Strong silhouettes and cleaner colour separation
- Animated water, foliage, signs, windows, and street lighting
- Soft canvas shadows and glow layered behind pixel sprites
- Polished life-sim UI while preserving the game's pixel identity

### Character direction

- Expanded modular sprites with more hairstyles and outfits
- Eight-direction movement where useful
- Facial emotes above characters during conversations
- Idle, sit, sleep, cook, talk, and celebrate animations
- Portrait illustrations for character creation and dialogue

### Environment direction

- Replace flat rectangles with reusable pixel tile sets
- Add roof shapes, façade details, pavement edges, and street props
- Give every district a distinct palette and visual landmarks
- Add interior wall variations and furniture sets
- Improve seasonal and weather overlays

### UI direction

- Pixel-framed panels with modern spacing
- Larger inventory and relationship portraits
- Animated need bars and mood indicators
- Clear interaction prompts anchored to world objects

### Technical approach

Retain the current camera, collision, scene structure, and tile coordinates. Replace drawing functions incrementally and introduce small sprite atlases stored directly in the project.

### Scope and risk

| Measure | Estimate |
|---|---|
| Visual impact | High |
| Engineering effort | Medium |
| New art required | Medium-high |
| Risk to existing systems | Low |
| Best for | Improving the whole game without a major rewrite |

---

## Direction 3 - Illustrated Storybook City

Reimagine Moonlight City as a hand-painted interactive storybook. Characters remain simple but gain bold outlines, soft shading, and expressive portraits. Environments become layered illustrations rather than tile-heavy scenes.

### Visual identity

- Hand-painted backgrounds with paper texture
- Bold, slightly imperfect outlines
- Pastel daylight and luminous night colours
- Page-turn transitions between interiors and districts
- Decorative stars, moons, flowers, and hand-lettered signs
- Character emotions shown through illustrated portrait cards

### Character direction

- Stylized chibi proportions with large faces and small bodies
- Swappable hair, face, outfit, and accessory layers
- A small set of highly expressive poses
- Illustrated close-up portrait during important social moments

### Environment direction

- Each district becomes a layered illustrated scene
- Parallax foreground and background elements create depth
- Buildings use unique hand-drawn façades
- Interiors emphasize personality over architectural accuracy
- Dungeon areas use inkier outlines and dramatic pools of light

### UI direction

- Notebook and sticker-inspired panels
- Hand-drawn icons with subtle bounce animation
- Dialogue presented like illustrated story cards
- Warm paper panels over colourful environments

### Technical approach

Keep canvas gameplay but replace most procedural environment drawing with layered image assets. Continue using existing collision and game logic beneath the new art.

### Scope and risk

| Measure | Estimate |
|---|---|
| Visual impact | Very high |
| Engineering effort | Medium-high |
| New art required | Very high |
| Risk to existing systems | Medium |
| Best for | A unique narrative and family-friendly identity |

---

## Comparison

| Direction | Biggest strength | Main tradeoff | Sims-inspired feeling |
|---|---|---|---|
| Moonlit Dollhouse | Strongest life-simulation presentation | Requires new projection and character systems | Strongest |
| Deluxe Pixel Life | Safest upgrade path | Less dramatic structural change | Moderate |
| Illustrated Storybook City | Most distinctive personality | Requires many bespoke illustrations | Light |

## Recommendation

Choose **Moonlit Dollhouse** for the largest transformation. It supports expressive characters, household storytelling, readable object interactions, and a premium life-simulation feel without requiring a full 3D engine.

Use **Deluxe Pixel Life** if preserving the current map and rapidly improving the whole game is more important than changing perspective.

Choose **Illustrated Storybook City** if the parent-and-child identity and narrative warmth should lead the art direction.

## Shared rules for every direction

1. Create original assets and interface components.
2. Keep controls readable for keyboard and mouse.
3. Preserve save compatibility and gameplay systems.
4. Upgrade one scene at a time behind a graphics-version flag.
5. Maintain consistent character identity across city and Moonlight Life.
6. Support 1280x720 first, then adapt layouts for smaller screens.
7. Measure performance before replacing the next scene.

## Suggested implementation order

1. Produce one character turnaround and one room mock-up.
2. Build the shared palette, typography, buttons, panels, and icon style.
3. Upgrade Create Your Life as the visual benchmark.
4. Upgrade the Moonlight Life household scene.
5. Apply the approved language to the title and world-selection screens.
6. Upgrade one city block as a compatibility test.
7. Continue through city interiors and the dungeon only after review.

This order creates an early playable art target without forcing a risky whole-game rewrite.
