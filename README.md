# COSMOS — Interactive Universe Simulator

A real-time, GPU-rendered galaxy and black hole simulator built entirely in a single HTML file. No build tools, no frameworks — just Three.js, custom GLSL shaders, and raw physics.

Check it out: https://space.atticusassociates.com

![COSMOS](https://img.shields.io/badge/Three.js-r160-blue) ![license](https://img.shields.io/badge/license-MIT-brightgreen)

---

## Features

### Galaxy
- **100,000 particles** rendered entirely on the GPU via custom GLSL vertex/fragment shaders
- **Differential rotation** — inner particles orbit slower than outer ones, creating a realistic vortex
- **Speed-based color** — black → deep blue → royal blue → violet → crimson → red, purely as a function of orbital radius
- **Swirl effect** — sinusoidal curl displacement gives each particle a continuous corkscrew drift
- **3D disc profile** — Gaussian height envelope: thick galactic bulge at the core, razor-thin at the rim
- **Brownian motion** — three incommensurable sine frequencies per particle per axis for organic jitter
- **Spiral arms** — two-arm structure with SPIN winding and brightness variation

### Black Hole
- **Pitch-black event horizon** — custom fragment shader renders pure black, no bloom bleed
- **Bipolar relativistic jets** — 7,000 particles erupting perpendicular to the disk, white-hot at the base fading to deep red
- **BH repulsion** — particles arc around the event horizon rather than being absorbed
- **Proximity darkening** — particles approaching the BH fade from blue to black

### Rocket
- **Tilted elliptical orbit** — Keplerian inverse-square gravity, 45° inclined to the galactic disk
- **Dashed trail** — records up to 420 positions, rendered with `LineDashedMaterial`
- **Autopilot** — stores the initial orbital plane normal; corrects back to it after gravity perturbation
- **Arrow key steering** — pitch and yaw the rocket in real time; autopilot updates to new heading
- **Exhaust plume** — bloom-lit cone that brightens and flickers when the autopilot is fighting gravity
- **Event horizon** — rocket is permanently swallowed if it crosses r ≤ 2.8 units


### Visuals
- **UnrealBloomPass** post-processing for physically-based glow
- **Three color themes** — Nebula Blue, Crimson Rift, Emerald Void
- **Nebula sprites** — soft billboarded quads behind the disk for depth
- **Cinematic vignette** — CSS radial gradient frame overlay
- **FPS counter** and particle count HUD

---

## Running Locally

Because this uses ES module imports via importmap, it **must** be served over HTTP (not opened as `file://`):

```bash
cd cosmos
python3 -m http.server 8888
# then open http://localhost:8888
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Rendering | [Three.js r160](https://threejs.org) via ES module CDN |
| Shaders | Custom GLSL (vertex + fragment) for galaxy, jets, BH |
| Post-processing | `EffectComposer` + `UnrealBloomPass` + `OutputPass` |
| Physics | CPU Euler integration, Keplerian gravity, autopilot spring |
| Deployment | [Vercel](https://vercel.com) — auto-deploys on push to `main` |

---

## Architecture

Everything lives in `index.html` — one self-contained file:

```
index.html
├── <style>          CSS (HUD, loader, d-pad, vignette)
└── <script type="module">
    ├── Galaxy       buildGalaxy()  — 100k particles, VERT/FRAG shaders
    ├── Black Hole   buildBlackHole() — pitch-black sphere, BH_VERT/BH_FRAG
    ├── Jets         buildJets()    — 7k particles, JET_VERT/JET_FRAG
    ├── Nebula       buildNebula()  — sprite quads
    ├── Rocket       updateRocket() — physics loop, trail, exhaust
    └── Render loop  EffectComposer + OrbitControls
```

---

## License

MIT
