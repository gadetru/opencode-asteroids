# AGENTS.md

## Project Overview

HTML5 Asteroids clone — a single-file vanilla JS game on an 800×600 Canvas. No frameworks, no bundler, no dependencies, no build step.

## Files

- `index.html` — page shell, loads `game.js`
- `game.js` — all game logic (~423 lines), ES6+ classes: `Ship`, `Asteroid`, `Bullet`, `Particle`
- `favicon.svg` — game icon
- `README.md` — Spanish-language description and controls

## Running

Open `index.html` directly in a browser, or use a local server:

```bash
npx serve .
```

Then visit `http://localhost:3000`.

## Key Architecture Notes

- **No build/test/lint pipeline** — this is a browser game, not a Node project. There is no `package.json`, no CI, and no test framework.
- **All game state** lives in module-level vars (`ship`, `bullets`, `asteroids`, `particles`, `score`, `lives`, `level`, `state`).
- **Game loop** uses `requestAnimationFrame` with fixed-capped `dt` (max 0.05s).
- **States**: `'playing' | 'dead' | 'gameover'`. On `gameover`, pressing Space restarts via `initGame()`.
- **Toroidal wrapping**: `wrap(v, max)` handles edge crossing on both axes.
- **Asteroid splitting**: size 3 → 2 → 1 (size 1 is immovable). Points per size: 20/50/100.
- **Ship invincibility**: 3 seconds after spawn/rebirth, rendered as blinking (draw skips every other frame).

## Controls

| Key | Action |
|-----|--------|
| `←` `→` | Rotate ship |
| `↑` | Thrust |
| `Space` | Shoot |

## Game Constants (in `game.js`)

- `ROT = 3.5` rad/s, `THRUST = 260` px/s², `DRAG = 0.987`
- `Bullet.SPEED = 520`, `Bullet.ttl = 1.1`
- `Ship.shootCooldown = 0.2`, `Ship.NOSE = 21`
- Ship collision uses `ship.radius + a.radius * 0.82` hit test

## Adding Features

New game objects should follow the existing class pattern: constructor with `this.dead = false`, `update(dt)`, and `draw()` methods. Append to the relevant array in the game loop. No import/export — everything shares the global scope.
