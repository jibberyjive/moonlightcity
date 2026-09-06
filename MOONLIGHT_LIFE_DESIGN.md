# Moonlight Life - Game Design Document

## High concept

**Moonlight Life** is a self-contained household simulation inside Moonlight City. It is inspired by the readable needs, queued actions, relationships, careers, skills, moods, and aspirations of modern life-simulation games while using original rules, names, interface, and content.

The player uses the same save and wallet as the main game, then creates a two-person playable household for this pack. Progress in Moonlight Life is saved in the existing browser save, but the mode has its own clock and does not alter the city, dungeon, or house.

The visual direction is an original, bright, readable life-simulation style. It may evoke the friendliness of games such as The Sims 4, but it does not copy another game's assets, interface, characters, branding, or exact visual design.

## Design goals

1. Make everyday choices feel playful and immediately understandable.
2. Let the player queue a short plan, watch it unfold, and change the next plan.
3. Reward balanced care without harsh failure states.
4. Connect the mode to Moonlight City through the player character and shared gold.
5. Keep every visual and system isolated so the rest of the game remains unchanged.
6. Do not include Build Mode in this pack.

## Access flow

`Title -> Continue -> Choose Your World -> Moonlight Life Pack -> Create Your Household`

The world-selection screen appears before the player enters Moonlight City. A first-time Moonlight Life player enters Create Your Life; returning players go directly to their household.

## Create Your Household

The pack has a dedicated household creator. Players design two characters, choose which member they are editing, and select their relationship:

- Name
- Skin tone
- Hair colour and style
- Outfit
- Body frame
- Aspiration
- Personality trait
- Relationship: Partners, Spouses, Siblings, or Best Friends

Both characters use the City Fashion HD renderer and remain independently playable. Their choices do not overwrite the main game's character.

Returning players can select Moonlight Life Pack on the world-selection screen and press **C** to edit their household.

## Core loop

`read needs -> choose a wish or urgent need -> queue actions -> watch actions resolve -> gain skills, bonds, gold, or aspiration -> plan the next day`

A useful play session lasts 3-10 minutes. The player should complete at least one wish, improve one skill or relationship, and leave with a clear next goal.

## Current playable scope

### Needs

| Need | Falls over time | Restored by |
|---|---:|---|
| Hunger | Fast | Cook a meal |
| Energy | Medium | Sleep |
| Fun | Medium | Play a game, chat |
| Social | Slow | Chat with Nova |
| Hygiene | Slow | Take a shower |

Needs range from 0 to 100. Low needs affect the visible mood but do not delete progress or end the game.

### Actions

| Action | Main result | Secondary result |
|---|---|---|
| Cook a meal | Restores Hunger | Raises Cooking |
| Take a shower | Restores Hygiene | Slight Energy gain |
| Sleep | Restores Energy | Advances the clock by 8 hours |
| Call a friend | Restores Social | Raises Charisma |
| Spend time together | Restores Social and Fun | Raises the household bond |
| Play a game | Restores Fun | Small Social cost |
| Study skills | Raises Logic | Costs Energy and Fun |
| Work a shift | Earns 90 shared gold | Costs several needs |

The player may queue up to four upcoming actions. The active action and queue are always visible.

### Moods

Mood is derived from the full needs profile:

- **Inspired** - average needs are exceptionally high.
- **Happy** - average needs are healthy.
- **Fine** - needs are stable.
- **Uncomfortable** - at least one need is low.
- **Miserable** - at least one need is critical.

Moods are feedback, not punishment. Future updates can use moods to unlock special interactions.

### Wishes and aspiration

One wish is shown at a time. Completing its matching action grants 25 Aspiration Points and reveals the next wish. Wishes rotate through the mode's major activities so new players naturally discover every system.

### Persistent progression

The mode saves:

- Household day and time
- Five needs
- Cooking, Charisma, and Logic skill progress
- Each member's individual needs and skills
- Household relationship type and shared bond
- Whether a child has joined the family
- Aspiration Points and wishes completed

Gold is shared with Moonlight City. A work shift in Moonlight Life can fund a city house or supplies, while the player keeps one recognizable identity across both games.

### Playable household and children

- Both designed household members appear in the apartment.
- Press **Tab** while the queue is empty to switch the active playable character.
- Each member has separate needs and skill progress.
- The moon marker and HUD identify the active character.
- Spouses unlock **Welcome a child** after reaching Household Bond 3.
- The child inherits visual choices from both parents, uses the child City Fashion HD template, has separate needs and skills, and becomes playable with Tab.

## Controls

| Input | Action |
|---|---|
| Up / Down | Select an action |
| Enter / E | Add selected action to the queue |
| Backspace | Remove the last queued action |
| Tab | Switch playable household member when the queue is empty |
| Escape | Save and return to world selection |

## Interface and visual boundary

Moonlight Life has a dedicated scene containing:

- A simple cutaway apartment
- Existing player character rendering
- Household object labels
- Action list and action queue
- Needs bars, mood, clock, gold, aspiration, and current wish

No existing city tiles, sprites, interiors, dungeon visuals, palettes, or shared rendering functions are modified. The pack adds only its world-selection screen, character creator, and household scene.

## Explicitly out of scope

- Build Mode
- Copied assets or exact interface designs from other games
- Changes to the appearance of Moonlight City's existing scenes

## Failure and accessibility rules

- There is no game over for depleted needs.
- Actions never fail randomly.
- Queued actions can be removed before they begin.
- Important state is communicated by text, colour, and numeric values.
- Controls are keyboard-only and use keys already supported by Moonlight City.
- Progress saves after completed actions, every 15 seconds, and on exit.

## Future expansion

1. Add careers with schedules, promotions, and choice events.
2. Turn skill points into levels with action unlocks.
3. Add relationship events and invitations involving Moonlight City NPCs.
4. Add mood-specific interactions and short-term personality traits.

These additions must remain inside the Moonlight Life scene unless a separate integration change is deliberately approved.
