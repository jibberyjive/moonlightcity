# Moonlight City - PC and NPC Sprite Redesign

## Purpose

Redesign every playable character and NPC around the selected **Deluxe Pixel Life** direction. Characters should feel expressive, fashionable, and easy to identify while remaining readable in Moonlight City's top-down canvas world.

The concepts are original. They use broad life-simulation principles such as expressive faces, modular outfits, strong silhouettes, and readable emotions without copying another game's characters, assets, animations, or exact style.

![Three character sprite concepts](./docs/character-sprite-concepts.svg)

## Current problems to solve

- Adult sprites mix large anime eyes with a compact farming-game body.
- Toddler, child, and adult proportions do not feel like one visual family.
- NPC identity depends heavily on colour instead of silhouette and accessories.
- Clothing has too little space for recognizable fashion details.
- Emotional state is mostly communicated through dialogue rather than animation.
- Character creation shows options, but face and outfit differences are limited.

## Concept A - Moonlight Heroes

**Recommended**

Use a **24 × 40 source sprite**, displayed at 2× scale in the city and 3–4× in portraits and character creation.

### Proportions

- Head: 20 × 20 pixels
- Torso: 14 × 11 pixels
- Legs: 6 × 8 pixels each
- Hands and shoes use exaggerated highlight pixels
- Eyes remain large enough to show mood without dominating the face

### Strengths

- Best balance between expression and world readability
- Enough space for jackets, dresses, uniforms, jewellery, and bags
- Works with the current collision boxes and 32-pixel map grid
- Supports distinct adult, child, and toddler variants
- Feels like a major upgrade without forcing map reconstruction

### NPC differentiation

Each named NPC receives:

- A unique hair silhouette
- One signature accessory
- A two-colour outfit palette
- A unique idle pose
- A small reaction animation

Examples: Ella adjusts her apron, Lumi checks a paintbrush, Rex folds his arms, Rose's coat moves in an unseen breeze, and Doc checks a clipboard.

## Concept B - City Fashion

Use a **24 × 48 source sprite** with taller bodies, smaller heads, and more clothing space.

### Proportions

- Head: 18 × 18 pixels
- Torso: 12 × 15 pixels
- Legs: 5 × 12 pixels each
- Narrower shoulders and longer silhouette

### Strengths

- Best fashion detail
- Strongest adult and teen differentiation
- Elegant appearance in character creation and dialogue portraits
- Supports coats, layered outfits, long dresses, and formal clothing

### Tradeoffs

- Faces are less readable during ordinary gameplay
- Existing doors, furniture, and interaction offsets need adjustment
- Toddler and child variants require more separate animation work

## Concept C - Cozy Chibi

Use a **24 × 32 source sprite** with large heads, short limbs, and bold facial expressions.

### Proportions

- Head: 22 × 20 pixels
- Torso: 15 × 8 pixels
- Legs: 6 × 5 pixels each
- Oversized hair, hats, and accessories

### Strengths

- Clearest emotions
- Strong parent-and-child appeal
- Excellent readability for outfits and collectible cosmetics
- Lowest animation cost

### Tradeoffs

- Less suitable for serious dungeon scenes
- Adult, teen, and child silhouettes are harder to distinguish
- Least compatible with the more detailed city environment

## Comparison

| Feature | Moonlight Heroes | City Fashion | Cozy Chibi |
|---|---:|---:|---:|
| Emotion readability | Excellent | Good | Excellent |
| Fashion detail | Very good | Excellent | Good |
| Current map compatibility | Excellent | Fair | Excellent |
| Age differentiation | Very good | Excellent | Fair |
| Animation workload | Medium | High | Low |
| Deluxe Pixel Life fit | Excellent | Very good | Good |

## Recommended production design

Choose **Moonlight Heroes**.

It preserves the friendly proportions of the current game while fixing the inconsistent style and providing enough pixels for expressive faces, fashion, and NPC-specific animation.

### Sprite layers

Render each character from reusable layers:

1. Ground shadow
2. Back hair
3. Legs and shoes
4. Torso outfit
5. Arms and hands
6. Neck and head
7. Eyes, brows, nose, and mouth
8. Front hair
9. Hat or accessory
10. Status effect or interaction marker

### Palette rules

- Maximum 16 colours per completed sprite
- Two shadow steps and one highlight step per material
- Dark blue-purple outlines instead of pure black
- Skin tones retain warmth under daytime and moonlight overlays
- Important NPC colours must remain distinguishable in greyscale
- Hair and outfit colours should not merge at the shoulders

### Required animation set

| Animation | Frames | Directions |
|---|---:|---:|
| Idle | 2 | 4 |
| Walk | 6 | 4 |
| Talk | 4 | 4 |
| Happy reaction | 4 | Front |
| Sad reaction | 4 | Front |
| Angry reaction | 4 | Front |
| Sit | 2 | Left/right |
| Sleep | 2 | Left/right |
| Use object | 4 | 4 |
| Attack | 5 | 4 |
| Hurt | 2 | 4 |

Diagonal movement can continue using the nearest cardinal animation. Eight-direction art should only be added if playtesting shows that four directions feel limiting.

## Age variants

### Adult

Uses the complete 24 × 40 frame and widest clothing selection.

### Child

Uses a 22 × 34 frame, slightly larger head ratio, shorter torso, and energetic idle movement. Child characters use age-appropriate clothing and interactions.

### Toddler

Uses a 20 × 27 frame, broad head, short limbs, slower walk cycle, and exaggerated reactions. Toddler sprites never reuse scaled adult bodies.

## Character creator improvements

- Show front, side, and back previews.
- Add eye shape, eyebrow, mouth, face shape, and accessory categories.
- Preview idle and walk animations.
- Display outfit colour swatches rather than numbered options.
- Keep body frame changes subtle enough to preserve animation alignment.
- Show the character against both daylight and moonlight backgrounds.

## Portrait system

Dialogue portraits should be generated from the same choices as the world sprite:

- 64 × 64 source portrait
- Neutral, happy, sad, surprised, and angry expressions
- NPC-specific pose or accessory
- Background colour based on relationship level
- Pixel border matching the Deluxe Pixel UI

This keeps customization consistent without requiring separately painted portraits for every possible player combination.

## Implementation plan

1. Build the Moonlight Heroes adult front-facing idle sprite.
2. Add four-direction idle and walk animation.
3. Convert the player and one NPC as a side-by-side test.
4. Add the layered appearance data model.
5. Convert remaining adults in small groups.
6. Build child and toddler templates.
7. Add emotion and interaction animations.
8. Add generated dialogue portraits.
9. Remove legacy sprite branches after every character has migrated.

## Acceptance criteria

- Player and NPCs share one coherent visual language.
- Every named NPC is identifiable without reading their name.
- Hair, skin, and outfit options remain visible at normal zoom.
- Existing collision and interaction distances still feel correct.
- Sprites remain crisp at integer scales.
- Character creation accurately previews gameplay appearance.
- City and dungeon animation stays smooth on the current browser target.
