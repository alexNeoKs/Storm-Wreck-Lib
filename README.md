# The Library of the Last Wyrm

A single-screen, Dungeon Master-controlled D&D encounter environment built in **Three.js** and **Vite**. It runs entirely in the browser and can be hosted on **GitHub Pages** without a backend.

The scene contains a monumental low-poly Bahamut-inspired dragon statue, 24 tall bookcase bays that rise toward a distant ceiling, four stone podiums at the statue, **1,000 drifting books** (GPU-instanced), and a grid-based stone platform connected to a long approach bridge.

## What's changed in this edition

- Header title is **The Library of the Last Wyrm**, with **−** to hide it and **Show title** to restore it. The "A tabletop encounter / Act I" subtitle was removed.
- Four named adventurers, each with its own procedural low-poly miniature: **Lethia** (female wizard with staff and hat), **Elsa** (ice princess with blue gown and blonde braid), **Gilchris** (male warlock with magical staff), and **Leda** (female halfling rogue with daggers). Elsa's design is a stylized primitive depiction, **not an imported or official Disney model**.
- The automated fairytale puzzle, spirits, books to match, and puzzle sidebar were removed.
- Five reusable enemy designs: **Enchanted Book**, **Red-Hooded Wolf**, **Ugly Goose**, **Man with a Flute**, **Boy with an Axe**.
- Spawn any number of enemies on any free tile, select them by clicking on the 3D piece or the right-hand enemy list, move them on the same grid as the party, remove one, or clear them all.
- Four decorative podiums surround the statue (not tied to puzzle logic).
- Dramatic full-screen **Success** and **Failure** flashes, controlled by the DM.
- A **5-tile-wide × 18-tile-long** bridge leads to the **15 × 15** central statue platform; movement works across the join.

> The tall library shelves also use stationary, instanced book-spine decoration. The 1,000 count refers to **the flying books**, as requested; it does not include the shelf decoration.

## Project layout

```text
bahamut-library/
├── index.html                   # Interface layout and controls
├── src/
│   ├── main.js                  # Scene, miniatures, enemies, movement, effects
│   └── style.css                # Layout, panels, flash overlay
├── package.json                 # Three.js and Vite dependencies
├── vite.config.js               # Relative URLs for GitHub Pages
├── .github/workflows/deploy.yml # GitHub Pages deployment workflow
├── .gitignore
└── README.md
```

## Run locally (Windows / macOS / Linux)

1. Install **Node.js 22 or newer** from https://nodejs.org/ (Node 20.19+ is also supported by this version of Vite).
2. Unzip the project and open the `bahamut-library` folder in VS Code.
3. Open a terminal in that directory, then run:

```bash
npm install
npm run dev
```

4. Visit the address displayed in your terminal, usually `http://localhost:5173/`.

To build a production version locally:

```bash
npm run build
npm run preview
```

The generated static site will be in `dist/`.

## Controls

| Control | Action |
| --- | --- |
| Left-drag the 3D view | Orbit the camera |
| Scroll | Zoom in/out |
| Right-drag | Pan |
| Explore | Return to dramatic camera view |
| Battle map | Overhead camera showing bridge and statue |
| Click hero or party entry | Select hero |
| Click enemy or enemy-list entry | Select enemy |
| Click glowing tile | Move selected piece on shortest valid orthogonal path (up to six squares) |
| Keys 1–4 | Select Lethia / Elsa / Gilchris / Leda |
| Choose Spawn on the right | Arm enemy placement, then click any free grid square |
| Escape | Cancel spawning / close help / dismiss the result overlay |
| Remove selected | Delete selected enemy |
| Clear all | Delete all enemies |
| Success / Failure | Large temporary full-screen result flash; click it to dismiss |
| − on title bar | Hide header to reveal the scene |
| Show title | Restore hidden header |
| Pause books | Freeze or resume the 1,000 drifting books |

The large statue occupies an inaccessible **5×5** central grid region. The four podium squares are also blocked. Characters cannot share occupied squares. Travel from the bridge to the statue platform is continuous.

## Deploy to GitHub Pages

### New repository

1. Create a repository named `bahamut-library` at https://github.com/new.
2. From the project root, run the following, replacing `YOUR-USERNAME` with your actual account name:

```bash
git init
git branch -M main
git add .
git commit -m "Add Library of the Last Wyrm"
git remote add origin https://github.com/YOUR-USERNAME/bahamut-library.git
git push -u origin main
```

3. Open your GitHub repository, go to **Settings → Pages**.
4. Set **Build and deployment → Source: GitHub Actions**.
5. Open **Actions** to see the deployment. If the workflow was triggered before Pages was enabled, manually rerun it.
6. When it finishes, open `https://YOUR-USERNAME.github.io/bahamut-library/`.

### Already have the original library repository?

Replace the existing `index.html`, `src/main.js`, `src/style.css`, and `README.md` with the revised versions, then:

```bash
git add .
git commit -m "Add bridge, hero models, enemy spawner and DM controls"
git push
```

The existing `.github/workflows/deploy.yml` will rebuild and deploy the new page. If you changed the repository name, `vite.config.js` already uses relative assets to avoid hard-coded paths.

## Modify the scene

In `src/main.js` the editable constants are near the top:

```js
const GRID_COUNT = 15;
const CELL = 1.55;
const MOVE_RANGE = 6;
const BOOK_COUNT = 1_000;
const BRIDGE_HALF_WIDTH = 2;
const BRIDGE_START = 8;
const BRIDGE_END = 25;
```

- `PARTY` changes player names, roles, colors and starting squares. The `style` values select the miniature constructors: `wizard`, `ice`, `warlock`, `rogue`.
- `ENEMY_TYPES` changes the five spawn options and their colors. Geometry is in `createEnemyMini()`.
- `PODIUMS` changes the podium coordinates. Those tiles become impassable.
- `createHall()`, `createShelves()`, `createBahamut()` handle the monumental environment.
- `createBoard()` builds the square platform and long bridge. To create a longer bridge, you should also ensure the library's surrounding shelves leave enough physical room for its new endpoint.
- `MOVE_RANGE` sets movement distance for both heroes and enemies.
- `flashResult(success)` powers the two dramatic cue buttons.

**Important:** The built-in miniature models use low-poly Three.js geometry and intentionally don't require external GLB files, textures or proprietary character assets. A detailed licensed or custom Elsa GLB would need to be sourced separately and swapped into `createPlayerMini()`.

## Deployment and gameplay limitations

- GitHub Pages is a **static host**. This is a **shared-screen / one-GM** interface, not synchronized multi-user play. Multiplayer requires a real-time service and authentication/state management.
- Enemy positions and puzzle outcomes are not persisted between refreshes.
- There is no turn-based initiative, health, attacks, dice, collision beyond tile occupancy or combat logic; the DM narrates outcomes.
- 1,000 flying books are animated by a single GPU-instanced mesh; the shelves use additional static instances. On slower mobile devices, reduce `BOOK_COUNT` further and lower `renderer.setPixelRatio`.

## Tech stack

- Three.js 0.186.1
- Vite 7.x
- Plain JavaScript / CSS / HTML
- GitHub Actions + GitHub Pages
