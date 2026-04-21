# Implementation Plan - PXL Sweep

This document outlines the phased approach for developing **"PXL Sweep,"** a high-fidelity Windows 95 Minesweeper recreation.

## Overview
PXL Sweep is a vanilla web application designed to evoke "UX Archeology" through frame-perfect recreation of classic Minesweeper logic and aesthetic. It features a 16x16 grid, classic Win95 styling, and a modern "CRT Twist" visual mode.

## Assumptions
- The project uses modern **ES Modules** (`type: module`) for organization during development.
- A minimal build or dev server (e.g., `sirv` or `live-server`) is used for local development.
- Testing is performed using a lightweight framework like **Vitest** or `Node:test` for logic-heavy modules (flood fill, neighbor calculation).
- The "single file" requirement in `REQUIREMENTS.md` is a preference; development will start modularly and can be bundled if necessary.

## Delivery Strategy
This plan uses a hybrid strategy:
- **Layered Infrastructure**: Initial phases focus on the "headless" game engine and base styling to ensure a solid foundation.
- **Vertical Slices**: Subsequent phases deliver functional features (Flood Fill, Chording, CRT Mode) that integrate logic, UI, and feedback in reviewable chunks.

This fits the project because the game logic is tightly coupled with its visual state, and the "Win95 Purist" goal requires pixel-perfect UI feedback at every step of interaction.

---

## Phase List
1. **Phase 1**: Project Setup & UI Shell
2. **Phase 2**: Headless Game Engine
3. **Phase 3**: Interactive Grid Rendering
4. **Phase 4**: Core Gameplay Logic (Flood Fill & Safety)
5. **Phase 5**: Game Meta & Smiley States
6. **Phase 6**: Timer & Mine Counter
7. **Phase 7**: The CRT Twist Module
8. **Phase 8**: Stabilization & Final Review

---

## Detailed Phases

### Phase 1: Project Setup & UI Shell
**Goal**: Establish the development environment and the static Windows 95 application container.

**Scope**:
- Initialize `package.json` with scripts for dev, test, and lint.
- Configure ESLint and Prettier for code consistency.
- Create the basic HTML5 structure and the "Win95 Window" CSS shell (title bar, gray background, 3D borders).

**Expected files to change**:
- `package.json`
- `index.html`
- `style.css`
- `.eslintrc.json`
- `TODO.md`

**Tests and checks**:
- `npm run lint`
- `npm run dev` (verify shell renders in browser)
- **Manual UX Check**: Does the window look like classic Windows 95?

**TODOs**:
- [ ] Initialize `package.json` with `type: module` and test scripts
- [ ] Setup ESLint/Prettier configuration
- [ ] Create `index.html` with basic game container structure
- [ ] Implement classic Win95 "outset" border CSS classes

**Exit Criteria**:
- `package.json` exists and `npm test` runs.
- `index.html` displays a window-like container with a title bar.
- CSS colors match `#c0c0c0` and `#808080` exactly.

---

### Phase 2: Headless Game Engine
**Goal**: Implement core logic for grid generation and mine calculation without DOM dependencies.

**Scope**:
- Grid initialization (16x16 array).
- Mine placement logic (40 mines).
- Neighbor count calculation logic.
- Data structure: `{ isMine, isRevealed, isFlagged, neighborCount }`.

**Expected files to change**:
- `js/engine.js`
- `tests/engine.test.js`

**Tests and checks**:
- Unit tests for `initializeGrid()`.
- Unit tests for `countNeighbors()`.
- Verify exactly 40 mines are placed.

**TODOs**:
- [ ] Define the Cell data structure
- [ ] Implement grid initialization (16x16)
- [ ] Implement random mine placement (40 mines)
- [ ] Implement neighbor calculation logic (1-8)
- [ ] Add unit tests for engine logic

**Exit Criteria**:
- Tests for neighbor calculation pass for all edge cases (corners, edges).
- 40 mines are verified in the grid state.

---

### Phase 3: Interactive Grid Rendering
**Goal**: Render the grid to the DOM and handle basic user clicks.

**Scope**:
- Dynamic generation of the 16x16 grid in the DOM.
- Visual mapping of cell states (Hidden, Revealed, Flagged).
- Left-click (reveal) and Right-click (flag) event handlers.

**Expected files to change**:
- `index.html`
- `js/main.js`
- `style.css`

**TODOs**:
- [ ] Render 16x16 grid dynamically from engine state
- [ ] Implement left-click handler for basic reveal
- [ ] Implement right-click handler for flagging (prevent context menu)
- [ ] Style tiles with Win95 "button" look (unrevealed vs revealed)

**Exit Criteria**:
- All 256 tiles are rendered and interactive.
- Right-click does not open the default browser menu.
- Tiles change visual state (inset vs outset) on click.

---

### Phase 4: Core Gameplay Logic (Flood Fill & Safety)
**Goal**: Implement recursive reveal and first-click safety.

**Scope**:
- Recursive flood fill algorithm for empty (0) tiles.
- **First-click safety**: Relocate mines if the first click is a mine and recalculate neighbors.

**Expected files to change**:
- `js/engine.js`
- `tests/engine.test.js`

**Risks**:
- Recursive functions can cause stack overflows; ensure proper boundary checks.

**TODOs**:
- [ ] Implement recursive flood fill for empty tiles
- [ ] Implement first-click safety logic (mine relocation)
- [ ] Add unit tests for flood fill boundaries

**Exit Criteria**:
- Flood fill stops exactly at numbered tiles.
- First click is never a mine in 20/20 manual attempts.

---

### Phase 5: Game Meta & Smiley States
**Goal**: Implement Win/Loss states and the reactive smiley face UI.

**Scope**:
- Detection of Win/Loss conditions.
- Smiley face component with state changes:
  - 🙂 (Idle/Reset)
  - 😮 (Mouse down on grid)
  - 😵 (Loss)
  - 😎 (Win)
- Board reset logic via smiley click.

**Expected files to change**:
- `js/main.js`
- `js/engine.js`
- `style.css`

**TODOs**:
- [ ] Implement Win/Loss state detection
- [ ] Create the reactive Smiley Face component
- [ ] Map mouse events to smiley state changes
- [ ] Implement reset logic on smiley click

**Exit Criteria**:
- Game state resets correctly.
- All mines are revealed on loss.
- Smiley reacts to mouse interactions in real-time.

---

### Phase 6: Timer & Mine Counter
**Goal**: Implement numeric displays using a 7-segment aesthetic.

**Scope**:
- 3-digit Timer (0-999) starting on first click.
- 3-digit Mine Counter (starts at 40, decrements on flag).
- CSS or Sprite-based 7-segment digit display.

**Expected files to change**:
- `js/main.js`
- `style.css`

**TODOs**:
- [ ] Create 7-segment digit display component
- [ ] Implement the decrementing Mine Counter
- [ ] Implement the incrementing Timer
- [ ] Ensure timer stops and holds value on game end

**Exit Criteria**:
- Mine counter accurately reflects flags placed.
- Timer starts on first click and stops on win/loss.

---

### Phase 7: The CRT Twist Module
**Goal**: Implement the vintage monitor simulation effects.

**Scope**:
- Toggle switch for "CRT Mode".
- CSS Scanline overlay and subtle flickering animation.
- Vignette/Curvature effect.

**Expected files to change**:
- `index.html`
- `style.css`
- `js/main.js`

**TODOs**:
- [ ] Create the CRT Mode toggle UI
- [ ] Implement CSS Scanline and Vignette overlays
- [ ] Implement the subtle flicker animation
- [ ] Ensure CRT overlays do not block mouse interaction (`pointer-events: none`)

**Exit Criteria**:
- CRT mode can be toggled without affecting game state.
- Visuals evoke a 90s monitor feel.

---

### Phase 8: Stabilization & Final Review
**Goal**: Resolve edge cases, implement chording, and finalize documentation.

**Scope**:
- **Chording Mechanic**: Dual-click reveal logic for speed-running.
- Final visual polish and pixel alignment.
- Documentation update.

**Expected files to change**:
- `js/main.js`
- `js/engine.js`
- `REQUIREMENTS.md`
- `README.md`

**TODOs**:
- [ ] Implement Chording (dual-click reveal) logic
- [ ] Finalize pixel-perfect CSS alignment
- [ ] Perform cross-browser testing
- [ ] Update documentation and project summary

**Exit Criteria**:
- All functional and UI requirements are verified.
- "Chording" works reliably for high-speed play.

---

## Dependency Notes
- **Headless logic (Phase 2)** must precede **DOM rendering (Phase 3)**.
- **First-click safety (Phase 4)** depends on the grid being interactive (Phase 3).
- Advanced features like **Chording** are reserved for the stabilization phase to ensure base stability first.

## Review Policy
- Each phase corresponds to one review cycle.
- If a phase implementation grows beyond 200 lines, it must be split into sub-tasks.
- No phase can be marked as "In Progress" in `TODO.md` until the previous phase is in `DONE.md`.

## Definition of Done
The project is complete when:
- A functional 16x16 Minesweeper game is playable in the browser.
- All "Windows 95 Purist" requirements (Smiley reactivity, 7-segment displays) are met.
- Logic is verified by automated or manual smoke tests. 