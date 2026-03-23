# Everything is Lava

A browser-based endless runner game where the floor is lava! Play as a ninja jumping between stone platforms while the lava rises beneath you. Built with vanilla HTML5 Canvas and JavaScript — no dependencies, no build step, just open and play.

## How to Play

Open `everything-is-lava.html` in any modern browser.

**Controls:**
- **Jump**: Space, W, or Up Arrow
- **Double Jump**: Press jump again while airborne
- **Move Left/Right**: A/D or Left/Right Arrows
- **Mobile**: Tap to jump

**Goal:** Survive as long as possible. The speed increases over time — see how far you can run before you melt!

## Features

- Procedurally generated platforms with increasing difficulty
- Double jump mechanic
- Animated lava surface with bubble particles
- Ninja character with flowing bandana tail, animated limbs, and angular mask eyes
- Screen shake and flash effects on death
- High score tracking (per session)
- Touch/click support for mobile
- Fully responsive canvas

## Tech Stack

- HTML5 Canvas API
- Vanilla JavaScript
- Zero dependencies
- Single-file architecture (~16 KB)

## Assets

- `assets/logo-get-good-dad-standard-blue.png` — Ninja logo used as reference for the player character design

## Build Prompts

This game was vibe-coded with Claude (Anthropic) in a single session. Below is every prompt used, in order, to produce the current version of the game.

### Prompt 1 — Initial Concept
```
let's make a game
```
*(Claude asked what kind of game — classic, puzzle, card, platformer, or something original.)*

### Prompt 2 — Game Identity
```
it a runner game called Everything is Lava
```
*(Claude built the complete v0.1.0: HTML5 Canvas runner with procedurally generated platforms, lava physics, double jump, particle effects, scoring, menu/death screens, keyboard + touch controls.)*

### Prompt 3 — Character Design
```
ok... decent concept, let's make a character from the assets folder
```
*(Claude read the ninja logo from `assets/logo-get-good-dad-standard-blue.png` and redesigned the player as a round blue ninja with angular mask eyes, flowing bandana tail, animated arms/legs, and matching blue particle effects.)*

### Prompt 4 — Documentation
```
document every change in a change log, add a robust readme that captures all of the prompts i use to create this game
```
*(Claude created CHANGELOG.md and this README.md.)*

## Project Structure

```
Everything is Lava/
├── everything-is-lava.html   # The game (single file, open in browser)
├── assets/
│   └── logo-get-good-dad-standard-blue.png  # Ninja logo reference
├── CHANGELOG.md              # Version history with detailed changes
└── README.md                 # This file
```

## License

Built for the Toledo.codes March Game Jam (Category B: Vibe-Coded). Theme: "Forward Motion."
