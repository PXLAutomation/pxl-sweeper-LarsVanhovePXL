# Implementation Blueprint: Phase 1 - Project Setup & UI Shell

## 1. Architectural Design
- **State Definitions**: No dynamic game state is required in this phase. The focus is strictly on static HTML/CSS layout and tooling configuration.
- **Data Structures**: None required yet.
- **Function Signatures**: None required yet.

## 2. File-Level Strategy
- `package.json`: Manages project dependencies (ESLint, Prettier, sirv-cli for dev server) and scripts (`dev`, `lint`, `format`, `test`). Explicitly sets `"type": "module"`.
- `.eslintrc.json`: Configures ESLint for browser and ES2021 environments, ensuring code consistency from the start.
- `.prettierrc`: Configures Prettier to ensure stylistic uniformity across HTML, CSS, and future JS files.
- `index.html`: The single-page container. Contains the semantic HTML structure for the Win95 window (Title Bar, Menu Bar, Game Header for Smiley/Timers, Grid Container).
- `style.css`: Implements the classic Windows 95 visual language. Defines the `#c0c0c0` background, pixelated fonts, and the crucial 3D "outset" and "inset" borders using CSS border properties.
- `TODO.md`: Will be updated with the specific execution steps for Phase 1 to track progress.

## 3. Atomic Execution Steps (Plan-Act-Validate)

**Task 1: Initialize `package.json` with `type: module` and test scripts**
- **Plan**: Create a standard `package.json` to define the project as an ES module and set up necessary NPM scripts for the development lifecycle.
- **Act**: Run `npm init -y`. Modify `package.json` to include `"type": "module"`. Add devDependencies for `sirv-cli` (dev server), `eslint`, and `prettier`. Add scripts: `"dev": "sirv . --dev"`, `"lint": "eslint ."`, `"format": "prettier --write ."`, `"test": "echo \"Error: no test specified\" && exit 1"`.
- **Validate**: Run `npm run dev` and ensure the server starts successfully. Check `package.json` syntax.

**Task 2: Setup ESLint/Prettier configuration**
- **Plan**: Create configuration files to enforce code style and catch early errors.
- **Act**: Create `.eslintrc.json` with recommended rules for vanilla browser JS. Create `.prettierrc` with basic formatting rules.
- **Validate**: Run `npm run lint`. It should pass (exit code 0) if no JS files exist or if they are valid.

**Task 3: Create `index.html` with basic game container structure**
- **Plan**: Scaffold the HTML skeleton reflecting a classic Win95 application window.
- **Act**: Create `index.html`. Add standard boilerplate. Link to `style.css`. Create a `.win95-window` container, `.title-bar`, `.menu-bar`, and the main `.game-container` (which will later hold the header and the grid).
- **Validate**: Open `index.html` in a browser and inspect the DOM structure to ensure semantic correctness.

**Task 4: Implement classic Win95 "outset" border CSS classes**
- **Plan**: Define the CSS variables and utility classes required for the Windows 95 aesthetic.
- **Act**: Create `style.css`. Define root variables for colors (`--win-bg: #c0c0c0`, etc.). Apply base styles to `body`. Create utility classes like `.outset-border` (white top/left, dark grey bottom/right) and `.inset-border` (dark grey top/left, white bottom/right). Style the `.title-bar`.
- **Validate**: Reload the browser. The UI must resemble a classic Windows 95 window with accurate 3D borders and colors. Verify no layout shifts occur.

## 4. Edge Case & Boundary Audit
- **CSS Border Box Sizing**: Failure to apply `box-sizing: border-box` globally will lead to unpredictable layout shifts when adding the thick Win95 borders. This must be applied to all elements.
- **Viewport Constraints**: The UI must not trigger scrolling on standard desktop resolutions. The `.win95-window` should be appropriately constrained (e.g., `display: inline-block` or centered with flexbox on the body).
- **Font Availability**: "MS Sans Serif" might not be available on all OSes. Fallbacks like `Arial, sans-serif` must be provided in the stack, though pixelated fonts are preferred for authenticity.
- **NPM Script Cross-Platform Compatibility**: Avoid complex shell commands in `package.json` scripts that might break on Windows vs. Linux/Mac environments. Keep scripts simple.

## 5. Verification Protocol
- [ ] **Automated**: Run `npm install` successfully.
- [ ] **Automated**: Run `npm run lint` (Should exit 0).
- [ ] **Automated**: Run `npm run format` (Should exit 0).
- [ ] **Manual**: Run `npm run dev` and navigate to the local server URL.
- [ ] **Manual UX Check**: Visually inspect the browser output. Verify:
  - Background of the window is exactly `#c0c0c0`.
  - The main window has a clear 3D outset border (white/light grey top-left, dark grey/black bottom-right).
  - The title bar is dark blue with white text and has a 'close' button placeholder.
  - The page has no unintended scrollbars.

## 6. Code Scaffolding

**`package.json` (Snippet)**
```json
{
  "name": "pxl-sweep",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "sirv . --dev --port 3000",
    "lint": "eslint .",
    "format": "prettier --write .",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "devDependencies": {
    "eslint": "^8.0.0",
    "prettier": "^3.0.0",
    "sirv-cli": "^2.0.0"
  }
}
```

**`index.html` (Snippet)**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PXL Sweep</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="win95-window outset-border">
        <div class="title-bar">
            <span class="title-text">Minesweeper</span>
            <button class="title-close outset-border">X</button>
        </div>
        <div class="menu-bar">
            <span>Game</span> <span>Help</span>
        </div>
        <div class="game-container inset-border">
            <!-- Header (Smiley/Timers) and Grid will go here in future phases -->
        </div>
    </div>
</body>
</html>
```

**`style.css` (Snippet)**
```css
:root {
    --win-bg: #c0c0c0;
    --win-border-light: #ffffff;
    --win-border-dark: #808080;
    --win-border-darker: #000000;
    --title-blue: #000080;
}

* {
    box-sizing: border-box;
}

body {
    background-color: teal; /* Classic desktop background */
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    font-family: 'MS Sans Serif', Arial, sans-serif;
    -webkit-font-smoothing: none; /* helps pixelated look on modern screens */
}

.outset-border {
    background-color: var(--win-bg);
    border-top: 2px solid var(--win-border-light);
    border-left: 2px solid var(--win-border-light);
    border-right: 2px solid var(--win-border-darker);
    border-bottom: 2px solid var(--win-border-darker);
    box-shadow: inset -1px -1px 0 var(--win-border-dark), inset 1px 1px 0 var(--win-bg);
}

.inset-border {
    border-top: 2px solid var(--win-border-darker);
    border-left: 2px solid var(--win-border-darker);
    border-right: 2px solid var(--win-border-light);
    border-bottom: 2px solid var(--win-border-light);
    box-shadow: inset 1px 1px 0 var(--win-border-dark);
}

.title-bar {
    background-color: var(--title-blue);
    color: white;
    padding: 2px 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: bold;
}
```