# Glycolysis 3D

An interactive, fully code-generated 3D visualization of **glycolysis** — the ten enzymatic
steps that break glucose down into pyruvate. Every molecule is built procedurally as a
ball-and-stick model with Three.js; there are no 3D model files, textures or images in the
project.

Scrub the timeline at the bottom to move through the pathway in either direction, at any
speed you like.

## Features

- **10 steps, 2 phases** — Energy-Investment and Energy-Payoff, each step with its enzyme,
  substrate, product and reaction equation.
- **Bidirectional timeline** — drag the progress bar forwards or backwards; every frame is a
  pure function of time, so seeking is always exact.
- **Ball-and-stick molecules** — pastel atoms (C, O, P, H) joined by rounded bonds,
  positioned by hand-written coordinate data.
- **Cofactor transformations** — `ATP → ADP` in steps 1 and 3, `ADP → ATP` in steps 7 and 10,
  `NAD⁺ → NADH` in step 6. Each cofactor is drawn once per molecule, so a ×2 step shows two
  separate pills, never a multiplier.
- **ATP ledger** — live net / consumed / produced ATP balance in the top-left corner
  (negative in red, positive in green).
- **No overlapping models** — a reactant fades out completely before its product fades in,
  and every cofactor pill occupies its own measured slot.
- **Minimal Q-style art direction** — flat cream background, toon shading, rounded labels,
  fixed camera.

## Spatial contract

The animation is intentionally constrained so that scrubbing never causes a change in depth:

- every molecule's geometric centre sits on the **same X axis** (`y = 0`, `z = 0`);
- the camera is fixed (it only reframes on window resize);
- the only spatial motion is translation along **X**, plus opacity fades.

The result reads as a horizontal conveyor belt rather than a fly-through.

## Getting started

```bash
npm install
npm run dev      # http://localhost:5173
```

Production build:

```bash
npm run build    # outputs static files to dist/
npm run preview
```

The build is a plain static site, so it can be hosted on GitHub Pages, Netlify, Vercel or any
static file server. `vite.config.js` sets `base: './'` so `dist/` works from a subdirectory.

## Controls

| Input | Action |
| --- | --- |
| Drag the progress bar | Scrub forwards / backwards |
| Click a step number (1–10) | Jump to that step |
| `Space` | Play / pause |
| `←` / `→` | Back / forward 3 seconds |
| `0`–`9` | Jump to step 1–9 (0 = step 10) |
| `?t=<seconds>` in the URL | Open the page at an exact moment, e.g. `?t=42.2` |

## Project structure

```
.
├── index.html          # page shell: phase banner, ATP ledger, equation panel, timeline
├── styles.css          # minimal Q-style UI styling
├── src/
│   ├── main.js         # scene, molecule builder, timeline orchestration, UI sync
│   └── molecules.js    # ball-and-stick coordinate data for the 11 compounds
├── vite.config.js
└── package.json
```

`src/molecules.js` holds the data for each compound — atom positions (`element, x, y, z`) and
bond pairs (`i, j`, optional bond order). Adding a molecule means adding one entry there.

## Scientific notes

- Net yield per glucose: **2 pyruvate, 2 ATP, 2 NADH** (4 ATP produced − 2 ATP invested).
- The ledger therefore reads −1 after step 1, −2 after step 3, back to 0 after step 7, and
  +2 after step 10.
- Steps 1, 3 and 10 are the irreversible, regulated reactions (hexokinase, PFK-1, pyruvate
  kinase).
- Molecular structures are simplified skeletons meant to convey composition and shape, not
  stereochemistry.

## Tech stack

- [Three.js](https://threejs.org/) — WebGL rendering, `MeshToonMaterial` spheres and cylinders
- [Vite](https://vitejs.dev/) — dev server and static build
- Plain ES modules, no framework

Labels are drawn to canvas at runtime with the [Quicksand](https://fonts.google.com/specimen/Quicksand)
web font (loaded from Google Fonts); the page still works with a fallback font when offline.

## License

[MIT](LICENSE)
