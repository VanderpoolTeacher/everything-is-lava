# Level 2-1: Volcano Climb — Design Spec

**Issue:** #1 — Add Level 2-1
**Date:** 2026-03-25
**Status:** Approved

## Overview

Level 2-1 is the first level of World 2 (the volcano arc). The player climbs a volcanic mountainside at an increasing incline (15° → 25°) through a destroyed city landscape. Trees replace streetlamps as platforms, molten rocks roll downhill as a new hazard, and lava rain continues from World 1.

## Terrain & Scrolling

### Angled Ground System

Replace the flat `GROUND_Y` constant with a `getGroundY(worldX)` function that computes ground height based on scroll position and incline angle.

- **Incline:** Starts at 15°, linearly ramps to 25° by end of level
- **Formula:** `groundY = baseGroundY - (worldX * tan(angle))` where angle lerps from 15° to 25° based on `levelTimer / levelDuration`
- **Clamp:** Ground never goes above ~40% of screen height
- **Scroll:** Horizontal scroll at `RUN_SPEED` (same as 1-x). The angled ground surface creates the climbing feel.
- **Camera & gravity:** Remain screen-relative (gravity pulls straight down). Only the terrain is angled.

- **Clamp behavior:** When the ground approaches the 40% mark, the effective angle smoothly flattens (reduces toward 0°) rather than hard-clamping. This avoids a visible kink in the terrain.
- **Ground rendering:** The ground is drawn as a filled polygon from the angled surface line down to the bottom of the canvas. Lava vein cracks follow the slope angle.

### GROUND_Y References — What Changes

Only gameplay-critical references to `GROUND_Y` change for `level === 4`. Rendering for levels 1-3, dojo mode, and menu screens must NOT be affected.

**Must use `getGroundY()`:**
- Player ground collision and spawn Y
- Lava drop ground-hit detection and lava pool spawn Y
- Tree base positioning
- Rock and molten rock Y tracking
- Player shadow Y position

**Must NOT change:**
- Dojo mode (uses its own layout)
- Menu/title screen
- Level 1-3 rendering functions (streetlamps, road, village, buildings)
- HUD/overlay elements

## Hazards

### Rocks (Physical Obstacles)

Non-lethal stone boulders that roll downhill from right to left. The primary rolling hazard in 2-1.

- **Sizes:** Varying — small (~24px), medium (~44px), large (~70px)
- **Speed:** Inversely proportional to size — small: 7–9 px/f, medium: 5–7 px/f, large: 3–5 px/f
- **Damage:** Knockback only — pushes the player left. Larger rocks push further (small: 30px, medium: 50px, large: 80px). No life lost.
- **Visual:** Gray/brown stone, no glow. Pixel-art style to match game aesthetic.
- **Movement:** Follow terrain slope via `getGroundY()`, rolling left (downhill)
- **Rotation:** Visual spin animation proportional to speed
- **Spawn rate:** Moderate — every ~100 frames in first third, every ~70 in second third, every ~50 in final third
- **Spawn position:** Right edge of screen, on the ground
- **Removal:** Roll off left edge of screen
- **Tree interaction:** Rocks roll on the ground and pass under/through trees

### Molten Rocks (Lethal)

Glowing lava-filled rocks that roll downhill. Rare in 2-1 — a deadly threat the player learns to distinguish from regular rocks.

- **Sizes:** Varying — same size range as rocks (small ~24px, medium ~44px, large ~70px)
- **Speed:** Same speed ranges as rocks per size
- **Damage:** Instant kill on any contact
- **Visual:** Glowing orange/red with lava cracks. Radial gradient from bright orange core to dark red edges. Emits faint glow/light.
- **Movement:** Follow terrain slope via `getGroundY()`, rolling left (downhill)
- **Rotation:** Visual spin animation proportional to speed
- **Spawn rate:** Very sparse — every ~200 frames in first third, every ~150 in second third, every ~100 in final third
- **Spawn position:** Right edge of screen, on the ground
- **Removal:** Roll off left edge of screen
- **Lava trail:** ~5% chance per frame to spawn a small lava pool (half normal pool size) behind the rock
- **Tree interaction:** Molten rocks roll on the ground and pass under/through trees (don't directly ignite trees, but their lava trail pools can)

### Lava Rain (Carried Over)

Same mechanic as 1-x levels with adjusted parameters:

- **Starting drop rate:** ~40 frames (harder than 1-3's starting rate of 44)
- **Ramps to:** ~20 frames by end of level
- **Lava pools:** Form on the angled ground (positioned via `getGroundY()`)
- **Tree interaction:** Lava drops or pools contacting a tree trigger the burning mechanic

## Platforms

### Trees (Replace Streetlamps)

Trees serve the same gameplay role as streetlamps in 1-x: the player jumps on top of them as platforms.

**Three fixed sizes:**

| Size | Height | Canopy Width | Jump Required | Distribution |
|------|--------|-------------|---------------|-------------|
| Small | ~80px | ~40px | Single jump | 30% |
| Medium | ~135px | ~55px | Double jump | 50% |
| Tall | ~180px | ~65px | Double jump from elevation | 20% |

**Properties:**
- **Spacing:** ~250px apart (tighter than lamp spacing of 300px)
- **Platform surface:** Top of canopy (player lands on crown of tree)
- **Trunk:** Passthrough — player can walk/fall through it
- **Visual style:** Pixel-art to match existing aesthetic — dark trunk, green canopy
- **Rooted to terrain:** Tree base positioned at `getGroundY(tree.worldX)`

### Burning Tree Mechanic

When lava (raindrop or pool) contacts a tree, it catches fire and becomes a timed hazard.

**Burn lifecycle (210 frames / ~3.5 seconds):**

1. **Normal** — Safe platform, green canopy
2. **Burning** (frames 0–150) — Flames animate upward from contact point, canopy darkens. Still standable.
3. **Critical** (frames 150–210) — Canopy flashes red/orange rapidly. Last warning.
4. **Collapsed** (frame 210) — Tree disappears. Falling ember/ash particle effect.

**Rules:**
- Player is NOT harmed by standing on a burning tree — only loses the platform on collapse
- **On collapse with player standing:** Player's `grounded` is set to `false`, gravity resumes, player falls. No damage, no special animation — they simply lose their footing. Implementation must check each frame whether the platform under the player still exists.
- Fire does NOT spread between trees
- Visual: flames spread upward from lava contact point, canopy color transitions green → brown → dark red

## Background

### Destroyed City (Replaces Neo-Tokyo)

Parallax layers depicting a city destroyed by lava, matching the existing parallax system:

| Layer | Speed | Content |
|-------|-------|---------|
| Sky | Fixed | Dark red/black gradient, drifting ash particles |
| Far (0.1x) | Slowest | Jagged ruined skyline silhouettes — broken rooftops, collapsed towers |
| Mid (0.35x) | Medium | Crumbled buildings with faint lava glow from cracks, flickering orange windows |
| Near (0.8x) | Fast | Rubble, broken walls, rising ember particles (replaces neon signs) |
| Ground (1.0x) | Full | Dark volcanic rock with glowing orange lava vein cracks, follows 15→25° incline |

**Atmospheric effects:**
- Smoke wisps: semi-transparent gray particles rising from random ground points
- Ash fall: small particles drifting downward (cosmetic only, no collision)

## Level Flow & Difficulty Curve

**Duration:** 3000 frames (50 seconds)

### First Third (frames 0–1000)
- Incline: 15°
- Lava rain: moderate (every ~40 frames)
- Rocks: moderate (every ~100 frames)
- Molten rocks: very rare (every ~200 frames)
- Trees: mostly medium, well-spaced
- Purpose: player learns the slope, trees, and distinguishes rock types

### Second Third (frames 1000–2000)
- Incline: ~20°
- Lava rain: increasing (every ~30 frames)
- Rocks: frequent (every ~70 frames)
- Molten rocks: occasional (every ~150 frames)
- Trees: mixed sizes, some burning from lava
- Purpose: pressure builds

### Final Third (frames 2000–3000)
- Incline: ~25°
- Lava rain: heavy (every ~20 frames)
- Rocks: heavy (every ~50 frames)
- Molten rocks: moderate (every ~100 frames)
- Trees: more tall ones, many burning
- Purpose: peak difficulty before level end

## Integration with Existing Systems

| Aspect | Detail |
|--------|--------|
| Internal level number | `level === 4` |
| Display name | "Level 2-1" |
| Duration | 3000 frames (50 seconds) — the existing formula `1800 + level * 300` naturally yields 3000 for level 4. Keep the formula. |
| Transition in | Standard level transition screen showing "Level 2-1" |
| Transition out | Standard `levelComplete` state (ready for 2-2) |
| Vehicles | None — rocks replace them |
| Streetlamps | None — trees replace them |
| Konami code | Press `4` after sequence to jump to 2-1 |
| Score base | Continues formula: `Math.floor(levelTimer / 3) + (level - 1) * 1000` = 3000 base for level 4 |
| Score completion bonus | Continues formula: `level * 1000` = +4000 for completing level 4 |
| Music | Reuse existing track (placeholder until new tracks added) |
| Lives | Carry over from 1-3 (or fresh 5 if entering via Konami) |
| Sound effects | Reuse existing SFX for now. No new sound effects required for 2-1. |

### Required Code Changes for Level Progression

These existing guards must be updated to allow level 4:

1. **Level cap in `startGame()`:** Change `level = Math.min(level + 1, 3)` to `Math.min(level + 1, 4)` (raise to 6 later when 2-2/2-3 are added)
2. **Victory check:** Change `if (level >= 3)` victory trigger to `if (level >= 4)` (raise later for more levels)
3. **Konami code handler:** Extend the digit check to include `Digit4` mapping to `level = 4`
4. **Level display:** Add formatting logic so `level === 4` displays as "Level 2-1" (formula: `World ${Math.ceil(level/3)}-${((level-1)%3)+1}` or just a lookup table)

### Visual Notes

- **Player sprite:** Player remains upright (no tilt to match slope). The screen-relative gravity makes this feel natural.
- **Tree spacing:** 250px minimum. Avoid placing two tall trees adjacent — if random selection picks tall, the next tree should be small or medium.

## What This Spec Does NOT Cover

- Levels 2-2, 2-3 (separate issues/specs)
- Medium and large rock variants (future levels)
- Knockback/stun mechanics (future levels)
- New music tracks (to be added separately)
- Story scroll content between worlds
