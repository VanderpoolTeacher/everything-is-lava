# Everything is Lava — Changelog

## v0.2.0 — Ninja Character Update (2026-03-23)

### Changed
- **Player character completely redesigned** as a ninja matching the `assets/logo-get-good-dad-standard-blue.png` logo
  - Round blue body with radial gradient shading
  - White angular mask across the eyes with dark angry eye cutouts (matching logo style)
  - Flowing dual-strand bandana tail that animates with wave physics
  - Animated ninja arms: raised in jumping pose, swinging while running
  - Small legs with feet that animate during running, tuck during jumps
  - Outer ring and inner ring detail matching the circular logo design
- **Particle effects recolored** to match ninja theme:
  - Jump particles changed from orange (`#ffaa44`) to blue (`#44bbff`)
  - Double jump particles changed from red (`#ff6622`) to cyan (`#66ddff`)
  - Death particles include blue ninja sparks (`#44ccff`) alongside lava orange
  - Trail particles changed from orange to light blue (`#66ccff`) smoke puffs

## v0.1.0 — Initial Build (2026-03-23)

### Added
- **Core game engine**: Canvas-based 2D runner with 60fps game loop
- **Player mechanics**:
  - Gravity-based physics (0.6 gravity, -12 jump force)
  - Double jump ability with on-screen indicator
  - Horizontal movement with A/D or Arrow Keys
  - Screen boundary clamping
  - Running animation with 4-frame cycle
- **Platform system**:
  - Procedurally generated stone platforms (60–160px wide)
  - Random vertical offset between platforms (±80px initially, ±100px for spawned)
  - Gap distance: 70–90px between platforms
  - Rounded rectangle rendering with gradient shading and crack textures
  - Platform shadow effect
  - Auto-cleanup of off-screen platforms and continuous spawning
- **Lava system**:
  - Animated wavy lava surface using layered sine waves
  - Gradient fill (red/orange lava colors)
  - Glow effect above the lava surface
  - Lava bubble particles that rise and fade
- **Visual effects**:
  - Player trail particles
  - Jump and death particle bursts
  - Screen shake on death
  - Flash effect on death
  - Twinkling stars in background
  - Distant volcano silhouettes
  - Dark volcanic sky gradient
- **Game states**: Menu screen, playing, and death screen
- **Scoring**: Distance-based scoring (meters), speed multiplier display, persistent high score
- **Difficulty scaling**: Speed increases over time (+0.0002 per frame)
- **Controls**: Keyboard (Space/W/Up to jump, A/D/Arrows to move) and touch/click support
- **UI**: Title screen with animated pulsing text, death screen with "YOU MELTED!" message, "NEW BEST!" indicator
- **Responsive canvas**: Scales to window size (max 800×500)
