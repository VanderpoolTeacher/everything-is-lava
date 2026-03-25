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

All collision checks that currently reference `GROUND_Y` must use `getGroundY(entityWorldX)` when `level === 4`.

## Hazards

### Molten Rocks

Small molten rocks roll downhill from right to left. Only small rocks appear in 2-1 — this introduces the mechanic before later levels add medium/large variants.

- **Size:** ~24px diameter
- **Speed:** 7–9 px/frame (fast, but jumpable)
- **Damage:** Instant kill on any contact
- **Movement:** Follow terrain slope via `getGroundY()`, rolling left (downhill)
- **Rotation:** Visual spin animation proportional to speed
- **Spawn rate:** Very sparse — every ~150 frames in first third, every ~100 in second third, every ~70 in final third
- **Spawn position:** Right edge of screen, on the ground
- **Removal:** Roll off left edge of screen
- **Lava trail:** Occasionally spawn small lava pools behind them

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
- Rocks: very rare (every ~150 frames)
- Trees: mostly medium, well-spaced
- Purpose: player learns the slope and tree mechanics

### Second Third (frames 1000–2000)
- Incline: ~20°
- Lava rain: increasing (every ~30 frames)
- Rocks: occasional (every ~100 frames)
- Trees: mixed sizes, some burning from lava
- Purpose: pressure builds

### Final Third (frames 2000–3000)
- Incline: ~25°
- Lava rain: heavy (every ~20 frames)
- Rocks: frequent (every ~70 frames)
- Trees: more tall ones, many burning
- Purpose: peak difficulty before level end

## Integration with Existing Systems

| Aspect | Detail |
|--------|--------|
| Internal level number | `level === 4` |
| Display name | "Level 2-1" |
| Duration | 3000 frames (50 seconds) |
| Transition in | Standard level transition screen showing "Level 2-1" |
| Transition out | Standard `levelComplete` state (ready for 2-2) |
| Vehicles | None — rocks replace them |
| Streetlamps | None — trees replace them |
| Konami code | Press `4` after sequence to jump to 2-1 |
| Score | Continues formula: `Math.floor(levelTimer / 3) + 3000` base |
| Music | Reuse existing track (placeholder until new tracks added) |
| Lives | Carry over from 1-3 (or fresh 5 if entering via Konami) |

## What This Spec Does NOT Cover

- Levels 2-2, 2-3 (separate issues/specs)
- Medium and large rock variants (future levels)
- Knockback/stun mechanics (future levels)
- New music tracks (to be added separately)
- Story scroll content between worlds
