# Level 2-1: Volcano Climb

## Overview

Level 2-1 introduces a new gameplay mechanic: the hero climbs the volcano at an approximately **15-degree incline**. The entire level scrolls upward at this angle as the player ascends toward the summit.

## Environment

- **Terrain**: Volcanic mountainside scrolling at ~15° incline
- **Background**: Smoke, ash, glowing lava veins in the rock face
- **Lava rain**: Continues falling from the sky (carried over from 1-x levels)

## New Hazards

- **Molten rocks**: Roll down the hill toward the player in varying sizes (small, medium, large). Larger rocks are slower but harder to jump over. Smaller rocks are faster. All are hurtboxes on contact.

## New Platforming

- **Trees of varying heights**: Scattered along the mountainside. Can be jumped on like the streetlamp posts in levels 1-1 through 1-3 (top = platform, side = passthrough or hurtbox TBD).
- **Burning trees**: If lava (rain drops or pools) hits a tree, it catches fire. A burning tree becomes a timed hazard — still standable briefly, but will collapse/disappear after a few seconds. Visual: flames spread from point of lava contact upward.

## Scroll Direction

- The level scrolls forward-and-up at ~15° rather than the flat horizontal scroll of levels 1-1 through 1-3.
- Camera and gravity remain screen-relative (gravity pulls straight down on screen), but the terrain itself is angled.

## Design Notes

- This is the first level of the volcano arc (World 2). The emerald story scroll at the end of 1-3 sets up the journey to the heart of the volcano.
- Difficulty should ramp from the end of 1-3 — the player is now an experienced runner.
- The incline creates a natural sense of "climbing" that reinforces the forward motion theme.
