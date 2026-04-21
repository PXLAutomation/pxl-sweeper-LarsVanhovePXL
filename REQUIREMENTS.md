# Requirements: Windows 95 Purist Minesweeper

## 1. Project Overview
The "Windows 95 Purist" Minesweeper is a high-fidelity web recreation of the classic 1995 logic puzzle. The objective is to evoke a sense of "UX Archeology"—making the user feel they are operating a legacy Windows application within a modern browser. The project follows a "Simple Complexity" philosophy: the code remains lean (Vanilla JS), but the execution of game logic and tactile feedback is frame-perfect.

## 2. Functional Requirements

### 2.1 Grid & Mine Logic
- **Grid Size**: Fixed 16x16 grid (Intermediate difficulty standard).
- **Mine Count**: 40 mines total.
- **First-Click Safety**: The first tile clicked by a user must *never* be a mine. If the initial coordinate is a mine, the mine must be relocated, and the board numbers recalculated before the reveal.
- **Number Calculation**: Each non-mine tile must display a number (1-8) representing the count of mines in the 8 adjacent squares. Empty tiles (0) remain blank.

### 2.2 Core Algorithms
- **Flood Fill**: When an empty tile (0) is clicked, the game must recursively reveal all contiguous empty and numbered tiles until a numeric boundary is met.
- **Chording**: If a user clicks a numbered tile that already has the correct number of flags surrounding it, the app must trigger a "chord" (dual-click action) to automatically reveal all remaining unflagged adjacent tiles.
- **Win/Loss States**: 
    - **Win**: All non-mine tiles are revealed.
    - **Loss**: A mine is clicked. All mines are revealed with a red background on the detonated mine.

## 3. UI/UX Specifications

### 3.1 Visual Language
- **Color Palette**: 
    - Background/Interface: `#c0c0c0` (Classic Windows Grey).
    - Borders: High-contrast `white` (top/left) and `#808080` (bottom/right) for the "outset" 3D effect.
- **Typography**: Pixelated sans-serif fonts (e.g., 'MS Sans Serif' or 'Courier').
- **Numbers**: Standard Minesweeper color coding (1: Blue, 2: Green, 3: Red, 4: Dark Blue, 5: Maroon, 6: Cyan, 7: Black, 8: Gray).

### 3.2 The Reactive Smiley Face
Located at the top-center of the interface:
- **Idle/Reset**: 🙂
- **Mouse Down (on grid)**: 😮
- **Game Over (Loss)**: 😵
- **Victory (Win)**: 😎
- **Action**: Clicking the face resets the board state and timer.

### 3.3 Displays
- **Timer**: 3-digit seven-segment display in the top right. Increments every second up to 999.
- **Mine Counter**: 3-digit seven-segment display in the top left. Decrements when a flag is placed; can go negative.

## 4. The "CRT Twist" Module
An optional toggle to simulate a vintage monitor:
- **Scanlines**: A CSS-based linear-gradient overlay with `pointer-events: none`.
- **Flicker**: A subtle opacity animation (0.01s – 0.03s variance) to mimic cathode-ray tube refresh rates.
- **Vignette**: Slight radial gradient to darken corners and simulate screen curvature.

## 5. Technical Constraints

### 5.1 Architecture
- **Tech Stack**: Vanilla HTML5, CSS3, and ES6+ JavaScript.
- **Dependencies**: Zero external libraries or frameworks (No React, jQuery, or Bootstrap).
- **File Structure**: Preferably a single `index.html` containing all styles and scripts for maximum portability.

### 5.2 State Management
- **Grid Representation**: A 2D array of objects.
- **Object Schema**: `{ isMine: bool, isRevealed: bool, isFlagged: bool, neighbors: int }`.
- **Event Handling**: Must distinguish between `mousedown`, `mouseup`, `contextmenu` (for flagging), and simultaneous left/right clicks (for chording).

## 6. Success Metrics
1. **Nostalgic Accuracy**: The 3D "inset/outset" borders behave exactly like Win95 buttons.
2. **Input Responsiveness**: No perceptible lag between clicks and tile reveals.
3. **Logic Integrity**: 100% accuracy in the Flood Fill and Chording mechanics.
4. **Static Stability**: The app fits entirely within a standard browser viewport without scrolling.

## 7.Project Setup Requirements
- The project shall include a `package.json` file in the repository root.
- The `package.json` file shall define the project's runnable commands in a consistent way.
- The `package.json` file shall include at least a `test` script so the same test command can be run every time.
- If the project uses ES module imports in JavaScript, `package.json` shall set `"type": "module"`.
- The project shall remain compatible with a plain JavaScript, static-site workflow.

# tech stack
plain javascript, html , css