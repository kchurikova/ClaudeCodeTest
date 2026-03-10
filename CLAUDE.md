# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A collection of browser-based games, each delivered as a **single self-contained HTML file** — no build step, no dependencies, no package manager. Open any file directly in a browser to play.

## Running a game

```bash
# Windows — opens in default browser
cmd.exe /c start "" "<game>.html"
```

## Git workflow

This repo uses Git with a remote on GitHub (`origin/master`). After every meaningful change:

1. Stage only the relevant file(s) by name (never `git add .`)
2. Write a concise, imperative commit message describing *what changed and why*
3. Push immediately: `git push`

## Architecture pattern

Every game follows the same pattern:

| Section | What it contains |
|---|---|
| `<style>` | All CSS, including Google Fonts import and any keyframe animations |
| HTML body | Minimal static scaffold (canvas, score display, buttons) |
| `<script>` | Entire game logic in an IIFE; no external JS |

**State is plain objects/arrays** — no frameworks, no classes. Game loops use `setInterval`; animation effects use `requestAnimationFrame` alongside the interval.

### Snake (`snake.html`)
- 20×20 grid, 24 px cells → 480×480 canvas
- State: `snake[]` (head at index 0), `dir`, `nextDir` (queued to prevent 180° reversal), `food`, `score`, `gameOver`, `started`
- Tick interval: 150 ms; `foodPulse` driven by `requestAnimationFrame` for the glow effect
- Three screens drawn on the same canvas: start, playing, game over

### Tic-Tac-Toe (`tictactoe.html`)
- DOM-based (no canvas); 9 `.cell` divs in a CSS grid
- State: `board[9]`, `current`, `over`, `scores {X, O, D}`
- Win detection checks all 8 lines on every move via a `WINS` constant

## Style conventions

- **70s retro theme** (snake): `#1a0a00` bg, `#6b8c42`/`#9dc840` snake, `#e8650a` food, `#f5c842` text, `Press Start 2P` font, CRT scanline overlay
- **Dark arcade theme** (tictactoe): `#1a1a2e` bg, `#e94560` X, `#a8dadc` O, `Segoe UI` font
- New games should establish their own cohesive visual theme and document the palette in comments at the top of the `<style>` block
