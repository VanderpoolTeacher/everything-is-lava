# Level 2-1: Volcano Climb — Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add Level 2-1 (Volcano Climb) to the game — an inclined terrain level with trees, rolling rocks, molten rocks, and a destroyed city background.

**Architecture:** Extend the existing single-file game (`index.html`) by adding `level === 4` branches throughout the update/render pipeline. A new `getGroundY(worldX)` function replaces flat `GROUND_Y` for level 4. New object arrays (`trees`, `rocks`, `moltenRocks`) follow the same patterns as existing `streetLamps` and `vehicles`. New parallax drawing functions handle the destroyed city background.

**Tech Stack:** HTML5 Canvas, Vanilla JavaScript (no dependencies)

**Spec:** `docs/superpowers/specs/2026-03-25-level-2-1-volcano-climb-design.md`

**Note:** This is a single-file canvas game with no test framework. Verification is visual — run the game in a browser, use Konami code (↑↑↓↓←→←→BA then press 4) to jump to level 2-1, and check behavior. Each task ends with a commit.

---

## Chunk 1: Level Progression & Terrain Foundation

### Task 1: Update Level Progression Guards

Open level 4 path through the existing level system.

**File:** `index.html`

- [ ] **Step 1: Raise level cap in startGame()**

At line 1251, change:
```js
level = Math.min(level + 1, 3);
```
to:
```js
level = Math.min(level + 1, 4);
```

- [ ] **Step 2: Raise victory check**

At line 1374, change:
```js
if (level >= 3) {
```
to:
```js
if (level >= 4) {
```

- [ ] **Step 3: Extend Konami code handler**

At line 1051, change:
```js
const levelNum = e.code === 'Digit1' ? 1 : e.code === 'Digit2' ? 2 : e.code === 'Digit3' ? 3 : 0;
```
to:
```js
const levelNum = e.code === 'Digit1' ? 1 : e.code === 'Digit2' ? 2 : e.code === 'Digit3' ? 3 : e.code === 'Digit4' ? 4 : 0;
```

- [ ] **Step 4: Update level display text**

At line 4497, change:
```js
ctx.fillText(`LEVEL ${level}`, W / 2, H / 2 - 20);
```
to:
```js
const levelDisplay = level <= 3 ? `LEVEL ${level}` : `LEVEL ${Math.ceil(level/3)}-${((level-1)%3)+1}`;
ctx.fillText(levelDisplay, W / 2, H / 2 - 20);
```

At line 3860, change:
```js
ctx.fillText(`Level ${level + 1} awaits...`, W / 2, H / 2 + 35);
```
to:
```js
const nextLevelDisplay = (level + 1) <= 3 ? `Level ${level + 1}` : `Level ${Math.ceil((level+1)/3)}-${((level)%3)+1}`;
ctx.fillText(`${nextLevelDisplay} awaits...`, W / 2, H / 2 + 35);
```

- [ ] **Step 5: Verify — open game, use Konami code, press 4**

The game should enter a playable state (it'll look like level 3 since we haven't added 2-1 content yet, but it shouldn't crash). Level complete should show "Level 2-1" text.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: open level progression to level 4 (2-1)"
```

---

### Task 2: Add getGroundY() Function & Wire Up Core Collisions

Introduce the angled terrain function and connect it to player physics.

**File:** `index.html`

- [ ] **Step 1: Add getGroundY() function and level 4 state variables**

Insert after the `GROUND_Y` constant (line 55). Add:
```js
// Level 2-1 terrain
function getGroundY(worldX) {
  if (level !== 4) return GROUND_Y;
  const progress = Math.min(levelTimer / levelDuration, 1);
  const angleDeg = 15 + progress * 10; // 15° → 25°
  const angleRad = angleDeg * Math.PI / 180;
  // worldX relative to scroll creates the slope — further right = higher ground
  const relativeX = worldX - scrollOffset;
  // Ground rises from left to right (right side is "uphill")
  const slopeOffset = (W - relativeX) * Math.tan(angleRad);
  // Smoothly flatten as we approach 40% screen height
  const minGroundY = H * 0.4;
  const rawY = GROUND_Y - slopeOffset;
  if (rawY < minGroundY) {
    // Smooth blend instead of hard clamp
    const excess = minGroundY - rawY;
    return minGroundY + excess * 0.3; // Asymptotically approaches minGroundY
  }
  return rawY;
}
```

- [ ] **Step 2: Wire player ground collision**

At line 1587, change:
```js
if (player.y + player.h >= GROUND_Y) {
    player.y = GROUND_Y - player.h;
```
to:
```js
const playerGroundY = getGroundY(player.x + scrollOffset);
if (player.y + player.h >= playerGroundY) {
    player.y = playerGroundY - player.h;
```

- [ ] **Step 3: Wire player spawn positions**

At line 1295 (startGame), change:
```js
player.y = GROUND_Y - SPRITE_H;
```
to:
```js
player.y = getGroundY(150 + scrollOffset) - SPRITE_H;
```

At line 1327 (respawnPlayer), change:
```js
player.y = GROUND_Y - SPRITE_H;
```
to:
```js
player.y = getGroundY(150 + scrollOffset) - SPRITE_H;
```

- [ ] **Step 4: Wire player shadow**

Find where the player shadow is drawn (in drawPlayer, approximately line 3536). Update the shadow Y to use `getGroundY(player.x + scrollOffset)` instead of `GROUND_Y` when `level === 4`.

- [ ] **Step 5: Wire lava drop ground detection**

In the lava drop update logic (around line 962), where drops check if they've hit the ground (`y >= GROUND_Y` or similar), update to use `getGroundY(drop.worldX)`.

- [ ] **Step 6: Wire lava pool rendering Y**

In drawLavaPools (around line 2603), where `GROUND_Y` is used for row positioning, update to use `getGroundY(pool.worldX)` when `level === 4`.

- [ ] **Step 7: Verify — Konami to level 4, player should walk on angled ground**

The ground collision should create a visible slope effect. The player should be higher on the right side of the screen than the left. Lava drops should land on the slope.

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "feat: add getGroundY() angled terrain for level 2-1"
```

---

### Task 3: Draw Angled Ground Surface

Replace the flat road with volcanic terrain rendering for level 4.

**File:** `index.html`

- [ ] **Step 1: Add drawVolcanicGround() function**

Insert near drawStreet() (after line 2537). Add a new function that draws the angled volcanic ground:
```js
function drawVolcanicGround() {
  const PX = 4;
  // Draw filled polygon from slope line to bottom of canvas
  ctx.beginPath();
  ctx.moveTo(0, getGroundY(scrollOffset)); // left edge ground Y
  // Sample points across the screen width
  for (let sx = 0; sx <= W; sx += PX) {
    ctx.lineTo(sx, getGroundY(sx + scrollOffset));
  }
  ctx.lineTo(W, H);
  ctx.lineTo(0, H);
  ctx.closePath();
  ctx.fillStyle = '#3a2515';
  ctx.fill();

  // Top surface line (lighter volcanic rock)
  ctx.beginPath();
  ctx.moveTo(0, getGroundY(scrollOffset));
  for (let sx = 0; sx <= W; sx += PX) {
    ctx.lineTo(sx, getGroundY(sx + scrollOffset));
  }
  ctx.strokeStyle = '#5a3a28';
  ctx.lineWidth = 3;
  ctx.stroke();

  // Lava vein cracks (animated)
  const veinSeed = Math.floor(scrollOffset / 200);
  for (let i = 0; i < 8; i++) {
    const hash = ((veinSeed + i) * 7919) & 0xFFFF;
    const sx = ((hash % W) + W) % W;
    const gy = getGroundY(sx + scrollOffset);
    const veinLen = 20 + (hash % 40);
    const progress = Math.min(levelTimer / levelDuration, 1);
    const angleDeg = 15 + progress * 10;
    const angleRad = angleDeg * Math.PI / 180;

    ctx.beginPath();
    ctx.moveTo(sx, gy + 4);
    ctx.lineTo(sx + veinLen * Math.cos(angleRad), gy + 4 + veinLen * Math.sin(angleRad) * 0.3);
    ctx.strokeStyle = `rgba(255, ${68 + (hash % 50)}, 0, ${0.3 + (Math.sin(frameCount * 0.03 + i) * 0.2)})`;
    ctx.lineWidth = 1 + (hash % 2);
    ctx.stroke();
  }
}
```

- [ ] **Step 2: Conditionally call drawVolcanicGround() instead of drawStreet()**

In the draw() function (around line 4577), change:
```js
drawStreet();
```
to:
```js
if (level === 4) {
  drawVolcanicGround();
} else {
  drawStreet();
}
```

- [ ] **Step 3: Verify — level 4 should show angled volcanic ground with lava veins**

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add volcanic ground rendering for level 2-1"
```

---

## Chunk 2: Trees & Burning Mechanic

### Task 4: Add Tree Generation & Rendering

Create the tree platform system that replaces streetlamps.

**File:** `index.html`

- [ ] **Step 1: Add trees array and generation function**

Insert near the streetLamps declaration (around line 674):
```js
let trees = [];
const TREE_SPACING = 250;

function generateTrees() {
  trees = [];
  let lastWasTall = false;
  for (let wx = 200; wx < 10000; wx += TREE_SPACING) {
    let sizeRoll = Math.random();
    let size;
    // Avoid two tall trees adjacent
    if (lastWasTall) {
      size = sizeRoll < 0.4 ? 'small' : 'medium';
    } else {
      size = sizeRoll < 0.3 ? 'small' : sizeRoll < 0.8 ? 'medium' : 'tall';
    }
    lastWasTall = (size === 'tall');

    const heights = { small: 80, medium: 135, tall: 180 };
    const canopyWidths = { small: 40, medium: 55, tall: 65 };
    const canopyHeights = { small: 20, medium: 28, tall: 35 };
    const trunkWidths = { small: 8, medium: 12, tall: 16 };

    trees.push({
      worldX: wx,
      size: size,
      height: heights[size],
      canopyW: canopyWidths[size],
      canopyH: canopyHeights[size],
      trunkW: trunkWidths[size],
      burning: false,
      burnTimer: 0,
      collapsed: false
    });
  }
}
```

- [ ] **Step 2: Call generateTrees() in startGame() when level === 4**

In startGame() (around line 1300, after generateStreetLamps()), add:
```js
if (level === 4) {
  generateTrees();
}
```

- [ ] **Step 3: Add drawTrees() function**

Insert near drawStreetLamps(). Draw pixel-art trees:
```js
function drawTrees() {
  const PX = 4;
  for (const t of trees) {
    if (t.collapsed) continue;
    const sx = t.worldX - scrollOffset;
    if (sx < -100 || sx > W + 100) continue;

    const baseY = getGroundY(t.worldX);
    const trunkTop = baseY - t.height;
    const canopyTop = trunkTop - t.canopyH;

    // Trunk
    ctx.fillStyle = t.burning ? '#4a2210' : '#5a3a20';
    ctx.fillRect(sx - t.trunkW / 2, trunkTop, t.trunkW, t.height);

    // Canopy — color shifts when burning
    let canopyColor;
    if (!t.burning) {
      canopyColor = '#2d5a1e';
    } else if (t.burnTimer < 150) {
      // Burning phase: green → brown
      const burnProgress = t.burnTimer / 150;
      const r = Math.floor(45 + burnProgress * 60);
      const g = Math.floor(90 - burnProgress * 50);
      const b = Math.floor(30 - burnProgress * 20);
      canopyColor = `rgb(${r},${g},${b})`;
    } else {
      // Critical phase: flash red/orange
      canopyColor = Math.floor(frameCount / 4) % 2 === 0 ? '#8a2a0a' : '#cc4400';
    }

    // Draw canopy as ellipse using arc
    ctx.fillStyle = canopyColor;
    ctx.beginPath();
    ctx.ellipse(sx, trunkTop, t.canopyW / 2, t.canopyH, 0, 0, Math.PI * 2);
    ctx.fill();

    // Darker canopy outline
    ctx.strokeStyle = t.burning ? '#3a1a08' : '#1a3a10';
    ctx.lineWidth = 2;
    ctx.stroke();

    // Fire effect when burning
    if (t.burning && !t.collapsed) {
      const flameCount = t.burnTimer < 150 ? 3 : 6;
      for (let f = 0; f < flameCount; f++) {
        const fx = sx + (Math.random() - 0.5) * t.canopyW * 0.6;
        const fy = trunkTop - Math.random() * t.canopyH;
        const fs = 4 + Math.random() * 6;
        ctx.fillStyle = `rgba(255, ${100 + Math.random() * 100 | 0}, 0, ${0.6 + Math.random() * 0.3})`;
        ctx.fillRect(fx - fs/2, fy - fs, fs, fs);
      }
    }
  }
}
```

- [ ] **Step 4: Call drawTrees() conditionally in draw()**

In draw() function, after the ground drawing, add:
```js
if (level === 4) {
  drawTrees();
} else {
  drawStreetLamps();
}
```
Replace the existing `drawStreetLamps()` call with this conditional.

- [ ] **Step 5: Verify — level 4 should show trees of varying heights on the slope**

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add tree generation and rendering for level 2-1"
```

---

### Task 5: Add Tree Platform Collision

Let the player land on tree canopies.

**File:** `index.html`

- [ ] **Step 1: Add tree platform collision in update()**

In the update() function, after the streetlamp collision code (around line 1522), add a level 4 branch:
```js
// Tree platform collision (level 4)
if (level === 4) {
  for (const t of trees) {
    if (t.collapsed) continue;
    const tsx = t.worldX - scrollOffset;
    if (tsx < -100 || tsx > W + 100) continue;

    const baseY = getGroundY(t.worldX);
    const platTop = baseY - t.height - t.canopyH;
    const platBot = baseY - t.height;
    const halfW = t.canopyW / 2;

    if (
      player.vy >= 0 &&
      player.x + player.w > tsx - halfW &&
      player.x < tsx + halfW &&
      player.y + player.h >= platTop &&
      player.y + player.h <= platBot + player.vy + 4
    ) {
      player.y = platTop - player.h;
      player.vy = 0;
      player.grounded = true;
      player.jumping = false;
      player.doubleJumped = false;
    }
  }
}
```

- [ ] **Step 2: Skip streetlamp collision for level 4**

Wrap the existing streetlamp collision block (around lines 1502-1522) in:
```js
if (level < 4) {
  // ... existing streetlamp collision code ...
}
```

- [ ] **Step 3: Verify — player can jump onto and stand on tree canopies**

Test all three sizes: small (single jump), medium (double jump), tall (double jump from medium tree).

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add tree platform collision for level 2-1"
```

---

### Task 6: Add Burning Tree Mechanic

Trees catch fire from lava and collapse after 3.5 seconds.

**File:** `index.html`

- [ ] **Step 1: Add lava-tree contact detection in update()**

In the update loop, after lava drop ground collision (where lava pools are spawned), add:
```js
// Check lava drops hitting trees (level 4)
if (level === 4) {
  for (const t of trees) {
    if (t.burning || t.collapsed) continue;
    const tsx = t.worldX - scrollOffset;
    const baseY = getGroundY(t.worldX);
    const treeTop = baseY - t.height - t.canopyH;

    // Check lava drops
    for (const d of lavaDrops) {
      const dsx = d.worldX - scrollOffset;
      if (Math.abs(dsx - tsx) < t.canopyW / 2 + 10 && d.y >= treeTop && d.y <= baseY) {
        t.burning = true;
        break;
      }
    }

    // Check lava pools under the tree
    if (!t.burning) {
      for (const p of lavaPools) {
        const psx = p.worldX - scrollOffset;
        if (Math.abs(psx - tsx) < (p.w / 2 + t.trunkW / 2)) {
          t.burning = true;
          break;
        }
      }
    }
  }
}
```

- [ ] **Step 2: Add burn timer update in update()**

In the update loop, add:
```js
// Update burning trees (level 4)
if (level === 4) {
  for (const t of trees) {
    if (t.burning && !t.collapsed) {
      t.burnTimer++;
      if (t.burnTimer >= 210) {
        t.collapsed = true;
        // Spawn ember particles
        const tsx = t.worldX - scrollOffset;
        const baseY = getGroundY(t.worldX);
        for (let i = 0; i < 8; i++) {
          particles.push({
            x: tsx + (Math.random() - 0.5) * t.canopyW,
            y: baseY - t.height - Math.random() * t.canopyH,
            vx: (Math.random() - 0.5) * 2,
            vy: -1 - Math.random() * 2,
            life: 60 + Math.random() * 40,
            maxLife: 100,
            color: `rgb(255, ${100 + Math.random() * 100 | 0}, 0)`
          });
        }
      }
    }
  }
}
```

- [ ] **Step 3: Handle player on collapsing tree**

In the tree platform collision code (from Task 5), add a check: if the tree the player is standing on has `collapsed === true`, set `player.grounded = false`. Alternatively, since collapsed trees are skipped in collision, the player will naturally fall on the next frame when collision no longer finds the platform. Verify this works.

- [ ] **Step 4: Verify — lava hitting a tree causes fire, tree collapses after ~3.5s, player falls if standing on it**

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add burning tree mechanic for level 2-1"
```

---

## Chunk 3: Rocks & Molten Rocks

### Task 7: Add Rock Spawning & Physics

Rolling stone boulders that knock the player back.

**File:** `index.html`

- [ ] **Step 1: Add rocks array and spawn function**

Insert near the trees declaration:
```js
let rocks = [];

function spawnRock() {
  const sizeRoll = Math.random();
  let size, radius, speed;
  if (sizeRoll < 0.4) {
    size = 'small'; radius = 12; speed = 7 + Math.random() * 2;
  } else if (sizeRoll < 0.75) {
    size = 'medium'; radius = 22; speed = 5 + Math.random() * 2;
  } else {
    size = 'large'; radius = 35; speed = 3 + Math.random() * 2;
  }
  const knockback = { small: 30, medium: 50, large: 80 };
  rocks.push({
    worldX: scrollOffset + W + 50,
    radius: radius,
    speed: speed,
    size: size,
    knockback: knockback[size],
    rotation: 0,
    rotationSpeed: speed / radius
  });
}
```

- [ ] **Step 2: Add rock update logic in update()**

```js
// Spawn and update rocks (level 4)
if (level === 4) {
  // Spawn rate based on level progress
  const third = levelDuration / 3;
  let rockRate;
  if (levelTimer < third) rockRate = 100;
  else if (levelTimer < third * 2) rockRate = 70;
  else rockRate = 50;
  if (levelTimer % rockRate === 0 && levelTimer > 30) spawnRock();

  // Update rocks
  for (let i = rocks.length - 1; i >= 0; i--) {
    const r = rocks[i];
    r.worldX -= r.speed;
    r.rotation += r.rotationSpeed;

    // Remove if off screen left
    if (r.worldX - scrollOffset < -100) {
      rocks.splice(i, 1);
      continue;
    }

    // Player collision — knockback
    const rsx = r.worldX - scrollOffset;
    const rsy = getGroundY(r.worldX) - r.radius;
    if (
      player.alive &&
      !player.invincible &&
      player.x + player.w > rsx - r.radius &&
      player.x < rsx + r.radius &&
      player.y + player.h > rsy &&
      player.y < rsy + r.radius * 2
    ) {
      player.x -= r.knockback;
      player.x = Math.max(10, player.x);
      // Brief invincibility to prevent multi-hit
      player.invincible = 20;
      rocks.splice(i, 1);
    }
  }
}
```

- [ ] **Step 3: Add drawRocks() function**

```js
function drawRocks() {
  for (const r of rocks) {
    const sx = r.worldX - scrollOffset;
    if (sx < -50 || sx > W + 50) continue;
    const gy = getGroundY(r.worldX);
    const cy = gy - r.radius; // center Y

    ctx.save();
    ctx.translate(sx, cy);
    ctx.rotate(r.rotation);

    // Stone body
    ctx.beginPath();
    ctx.arc(0, 0, r.radius, 0, Math.PI * 2);
    ctx.fillStyle = '#6b5b4f';
    ctx.fill();

    // Cracks/detail
    ctx.strokeStyle = '#4a3a2e';
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(-r.radius * 0.3, -r.radius * 0.2);
    ctx.lineTo(r.radius * 0.2, r.radius * 0.3);
    ctx.moveTo(r.radius * 0.1, -r.radius * 0.4);
    ctx.lineTo(-r.radius * 0.1, r.radius * 0.1);
    ctx.stroke();

    // Highlight
    ctx.beginPath();
    ctx.arc(-r.radius * 0.2, -r.radius * 0.2, r.radius * 0.3, 0, Math.PI * 2);
    ctx.fillStyle = 'rgba(150, 140, 130, 0.3)';
    ctx.fill();

    ctx.restore();
  }
}
```

- [ ] **Step 4: Call drawRocks() in draw() for level 4**

Add `if (level === 4) drawRocks();` in the draw function, after trees and before the player.

- [ ] **Step 5: Clear rocks array in startGame()**

In startGame(), add: `rocks = [];`

- [ ] **Step 6: Verify — rocks roll downhill, player gets knocked back on contact**

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add rolling rocks with knockback for level 2-1"
```

---

### Task 8: Add Molten Rock Spawning & Physics

Lethal glowing rocks that kill on contact and leave lava trails.

**File:** `index.html`

- [ ] **Step 1: Add moltenRocks array and spawn function**

```js
let moltenRocks = [];

function spawnMoltenRock() {
  const sizeRoll = Math.random();
  let radius, speed;
  if (sizeRoll < 0.4) {
    radius = 12; speed = 7 + Math.random() * 2;
  } else if (sizeRoll < 0.75) {
    radius = 22; speed = 5 + Math.random() * 2;
  } else {
    radius = 35; speed = 3 + Math.random() * 2;
  }
  moltenRocks.push({
    worldX: scrollOffset + W + 50,
    radius: radius,
    speed: speed,
    rotation: 0,
    rotationSpeed: speed / radius,
    glowPhase: Math.random() * Math.PI * 2
  });
}
```

- [ ] **Step 2: Add molten rock update logic in update()**

```js
// Spawn and update molten rocks (level 4)
if (level === 4) {
  const third = levelDuration / 3;
  let moltenRate;
  if (levelTimer < third) moltenRate = 200;
  else if (levelTimer < third * 2) moltenRate = 150;
  else moltenRate = 100;
  if ((levelTimer + 50) % moltenRate === 0 && levelTimer > 60) spawnMoltenRock();

  for (let i = moltenRocks.length - 1; i >= 0; i--) {
    const mr = moltenRocks[i];
    mr.worldX -= mr.speed;
    mr.rotation += mr.rotationSpeed;
    mr.glowPhase += 0.1;

    // Remove if off screen left
    if (mr.worldX - scrollOffset < -100) {
      moltenRocks.splice(i, 1);
      continue;
    }

    // Lava trail — 5% chance per frame
    if (Math.random() < 0.05) {
      spawnLavaPool(mr.worldX, getGroundY(mr.worldX), true); // pass small flag
    }

    // Player collision — instant kill
    const msx = mr.worldX - scrollOffset;
    const msy = getGroundY(mr.worldX) - mr.radius;
    if (
      player.alive &&
      !player.invincible &&
      player.x + player.w > msx - mr.radius &&
      player.x < msx + mr.radius &&
      player.y + player.h > msy &&
      player.y < msy + mr.radius * 2
    ) {
      killPlayer();
      moltenRocks.splice(i, 1);
    }
  }
}
```

- [ ] **Step 3: Update spawnLavaPool to accept a small flag**

Modify the existing `spawnLavaPool` function (around line 734) to accept an optional `small` parameter. When `small` is true, use half width (`20 + Math.random() * 15`) and shorter life (`200 + Math.random() * 100`).

- [ ] **Step 4: Add drawMoltenRocks() function**

```js
function drawMoltenRocks() {
  for (const mr of moltenRocks) {
    const sx = mr.worldX - scrollOffset;
    if (sx < -50 || sx > W + 50) continue;
    const gy = getGroundY(mr.worldX);
    const cy = gy - mr.radius;

    // Glow effect
    ctx.save();
    ctx.shadowColor = '#ff6600';
    ctx.shadowBlur = 15 + Math.sin(mr.glowPhase) * 5;

    ctx.translate(sx, cy);
    ctx.rotate(mr.rotation);

    // Molten body
    ctx.beginPath();
    ctx.arc(0, 0, mr.radius, 0, Math.PI * 2);
    const grad = ctx.createRadialGradient(0, 0, 0, 0, 0, mr.radius);
    grad.addColorStop(0, '#ffaa00');
    grad.addColorStop(0.4, '#ff6600');
    grad.addColorStop(0.7, '#cc3300');
    grad.addColorStop(1, '#661100');
    ctx.fillStyle = grad;
    ctx.fill();

    // Lava crack lines
    ctx.strokeStyle = '#ffcc00';
    ctx.lineWidth = 1.5;
    ctx.beginPath();
    ctx.moveTo(-mr.radius * 0.5, 0);
    ctx.lineTo(mr.radius * 0.3, -mr.radius * 0.2);
    ctx.moveTo(0, -mr.radius * 0.4);
    ctx.lineTo(mr.radius * 0.1, mr.radius * 0.4);
    ctx.stroke();

    ctx.restore();
    ctx.shadowBlur = 0;
  }
}
```

- [ ] **Step 5: Call drawMoltenRocks() in draw() for level 4**

Add after `drawRocks()`.

- [ ] **Step 6: Clear moltenRocks array in startGame()**

In startGame(), add: `moltenRocks = [];`

- [ ] **Step 7: Ensure killPlayer() exists or reference the correct death handler**

Check the existing death mechanic — likely sets `player.alive = false` and triggers the death animation. Use the same function/pattern used when vehicles hit the player (around line 1719-1728).

- [ ] **Step 8: Verify — molten rocks glow, leave lava trails, kill player on contact**

- [ ] **Step 9: Commit**

```bash
git add index.html
git commit -m "feat: add molten rocks with lava trails for level 2-1"
```

---

## Chunk 4: Background & Polish

### Task 9: Add Destroyed City Background

Replace the Neo-Tokyo parallax layers with destroyed city for level 4.

**File:** `index.html`

- [ ] **Step 1: Add destroyed city building generation**

Add arrays and generation near the existing building arrays:
```js
let ruins_far = [];
let ruins_mid = [];
let ruins_near = [];

function generateRuins() {
  ruins_far = [];
  ruins_mid = [];
  ruins_near = [];

  // Far ruins — jagged silhouettes
  for (let x = 0; x < W + 600; x += 40 + Math.random() * 30) {
    const h = 40 + Math.random() * 80;
    // Jagged top — simulate broken roofline
    const jagged = Math.random() < 0.6;
    ruins_far.push({ x, w: 30 + Math.random() * 25, h, jagged });
  }

  // Mid ruins — crumbled buildings with glow
  for (let x = 0; x < W + 600; x += 50 + Math.random() * 40) {
    const h = 30 + Math.random() * 60;
    const hasGlow = Math.random() < 0.3;
    ruins_mid.push({ x, w: 35 + Math.random() * 30, h, hasGlow, glowPhase: Math.random() * Math.PI * 2 });
  }

  // Near ruins — rubble and broken walls
  for (let x = 0; x < W + 600; x += 60 + Math.random() * 50) {
    const h = 20 + Math.random() * 40;
    ruins_near.push({ x, w: 25 + Math.random() * 35, h });
  }
}
```

- [ ] **Step 2: Add drawDestroyedCitySky() function**

```js
function drawDestroyedCitySky() {
  // Dark volcanic sky
  const skyGrad = ctx.createLinearGradient(0, 0, 0, H);
  skyGrad.addColorStop(0, '#1a0505');
  skyGrad.addColorStop(0.4, '#3d1111');
  skyGrad.addColorStop(1, '#5a1a0a');
  ctx.fillStyle = skyGrad;
  ctx.fillRect(0, 0, W, H);

  // Ash particles (cosmetic)
  for (let i = 0; i < 20; i++) {
    const hash = (i * 7919 + Math.floor(frameCount * 0.3)) & 0xFFFF;
    const ax = hash % W;
    const ay = (hash * 3 + frameCount * 0.5) % H;
    ctx.fillStyle = `rgba(180, 160, 140, ${0.2 + (hash % 30) / 100})`;
    ctx.fillRect(ax, ay, 2, 2);
  }
}
```

- [ ] **Step 3: Add drawRuinsLayer() function**

```js
function drawRuinsLayer(ruins, parallaxSpeed, brightness, isNear) {
  const PX = 4;
  const offset = (scrollOffset * parallaxSpeed) % (W + 600);

  for (const r of ruins) {
    const sx = r.x - offset;
    const wrappedX = ((sx % (W + 600)) + (W + 600)) % (W + 600) - 100;
    if (wrappedX < -60 || wrappedX > W + 60) continue;

    const baseY = GROUND_Y; // Background uses flat baseline for parallax
    const color = `rgb(${42 + brightness * 30 | 0}, ${20 + brightness * 15 | 0}, ${20 + brightness * 15 | 0})`;

    ctx.fillStyle = color;
    if (r.jagged) {
      // Jagged roofline
      ctx.beginPath();
      ctx.moveTo(wrappedX, baseY);
      ctx.lineTo(wrappedX, baseY - r.h);
      ctx.lineTo(wrappedX + r.w * 0.3, baseY - r.h * 0.7);
      ctx.lineTo(wrappedX + r.w * 0.5, baseY - r.h * 0.9);
      ctx.lineTo(wrappedX + r.w * 0.7, baseY - r.h * 0.6);
      ctx.lineTo(wrappedX + r.w, baseY - r.h * 0.4);
      ctx.lineTo(wrappedX + r.w, baseY);
      ctx.closePath();
      ctx.fill();
    } else {
      ctx.fillRect(wrappedX, baseY - r.h, r.w, r.h);
    }

    // Lava glow from cracks (mid layer)
    if (r.hasGlow) {
      const glowIntensity = 0.3 + Math.sin(frameCount * 0.02 + r.glowPhase) * 0.15;
      ctx.fillStyle = `rgba(255, 68, 0, ${glowIntensity})`;
      ctx.fillRect(wrappedX + r.w * 0.3, baseY - r.h * 0.5, r.w * 0.15, r.h * 0.3);
    }

    // Rising embers (near layer)
    if (isNear && Math.random() < 0.02) {
      particles.push({
        x: wrappedX + Math.random() * r.w,
        y: baseY - r.h,
        vx: (Math.random() - 0.5) * 0.5,
        vy: -1 - Math.random(),
        life: 40 + Math.random() * 30,
        maxLife: 70,
        color: `rgb(255, ${100 + Math.random() * 100 | 0}, 0)`
      });
    }
  }
}
```

- [ ] **Step 4: Call generateRuins() in startGame() for level 4**

- [ ] **Step 5: Wire destroyed city rendering in draw()**

In the draw() function, wrap the existing parallax calls:
```js
if (level === 4) {
  drawDestroyedCitySky();
  drawRuinsLayer(ruins_far, 0.1, 0, false);
  drawRuinsLayer(ruins_mid, 0.35, 0.3, false);
  drawRuinsLayer(ruins_near, 0.8, 0.1, true);
} else {
  drawNeoTokyoSky();
  drawBuildingLayer(buildings_far, 0.1, 0);
  drawBuildingLayer(buildings_mid, 0.3, 0.3);
  drawNeonSigns();
  drawBuildingLayer(buildings_near, 0.5, 0.1);
}
```

- [ ] **Step 6: Verify — level 4 has dark red sky, ruined buildings, ash particles, lava glow**

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add destroyed city background for level 2-1"
```

---

### Task 10: Disable Vehicles & Streetlamps for Level 4

Ensure World 1 elements don't spawn in World 2.

**File:** `index.html`

- [ ] **Step 1: Skip vehicle spawning for level 4**

In the vehicle spawn schedule (around lines 1689-1704), wrap the entire block:
```js
if (level < 4) {
  // ... existing vehicle spawn logic ...
}
```

- [ ] **Step 2: Skip streetlamp generation for level 4**

In startGame(), the `generateStreetLamps()` call should be conditional:
```js
if (level < 4) {
  generateStreetLamps();
}
```

- [ ] **Step 3: Verify — no vehicles or streetlamps appear in level 4**

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: disable vehicles and streetlamps for level 2-1"
```

---

### Task 11: Add Smoke Wisps & Final Polish

Atmospheric effects and edge case handling.

**File:** `index.html`

- [ ] **Step 1: Add smoke wisps in drawVolcanicGround()**

At the end of `drawVolcanicGround()`, add:
```js
// Smoke wisps rising from ground
for (let i = 0; i < 5; i++) {
  const hash = ((Math.floor(scrollOffset / 300) + i) * 4391) & 0xFFFF;
  const sx = ((hash % W) + W) % W;
  const gy = getGroundY(sx + scrollOffset);
  const wispY = gy - 10 - Math.sin(frameCount * 0.02 + i * 1.5) * 15;
  const alpha = 0.1 + Math.sin(frameCount * 0.015 + i * 2) * 0.05;
  ctx.fillStyle = `rgba(120, 110, 100, ${alpha})`;
  ctx.beginPath();
  ctx.ellipse(sx, wispY, 8, 4, 0, 0, Math.PI * 2);
  ctx.fill();
}
```

- [ ] **Step 2: Ensure lava drop rate uses level 4 parameters**

Check the drop rate calculation (around line 1407). The existing formula `Math.max(15, 60 - level * 8 - levelTimer * 0.005)` for level 4 gives a starting rate of `60 - 32 = 28`, which is already harder than 1-3. Verify this aligns with the spec's target of ~40 starting. Adjust if needed:
```js
const dropRate = level === 4
  ? Math.max(20, 40 - levelTimer * 0.007)
  : Math.max(15, 60 - level * 8 - levelTimer * 0.005);
```

- [ ] **Step 3: Handle the "Level 2-1 awaits..." text on level 3 complete**

Ensure when level 3 completes, the "next level" text correctly says "Level 2-1 awaits..." instead of "Level 4 awaits..."

- [ ] **Step 4: Verify full playthrough — play through levels 1-3, then 2-1**

Test:
- Levels 1-3 are unchanged
- Level 2-1 loads after 1-3
- Angled terrain works and ramps 15→25°
- Trees spawn, burn, collapse
- Rocks knock back, molten rocks kill
- Destroyed city background renders
- Level complete works
- Score calculates correctly
- Konami code to level 4 works

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add atmospheric effects and polish for level 2-1"
```

---

### Task 12: Final Commit & Cleanup

- [ ] **Step 1: Review all changes**

Read through the diff to check for any issues:
```bash
git diff main..HEAD --stat
```

- [ ] **Step 2: Verify .superpowers is in .gitignore**

```bash
echo ".superpowers/" >> .gitignore
git add .gitignore
```

- [ ] **Step 3: Final commit if needed**

```bash
git add -A
git commit -m "chore: add .superpowers to gitignore"
```
