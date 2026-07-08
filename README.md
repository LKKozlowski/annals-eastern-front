# The Annals — a living kingdom in a single file

![The Annals](share.jpg)

A procedurally generated medieval realm that lives on its own — harvests and plagues, dynasties and civil wars, bandits, omens, and a dragon asleep on its hoard — while its chronicle writes itself in the margin. Watch it like a nature documentary, or reach in and turn the dials of fate.

**Play it: [annals-kingdom.netlify.app](https://annals-kingdom.netlify.app)**

Everything is one HTML file. No build step, no backend, no saves — the entire kingdom regenerates deterministically from a seed in the URL hash, so a link *is* a world: share `#s=your-seed-here` and the recipient forges the exact same realm, down to the last shepherd.

## What lives in it

- **A forged world** — terrain, rivers that carve valleys and end in meres, biomes, roads found by pathfinding, towns planned street by street, heraldry drawn house by house
- **A working economy** — seven goods, prices from scarcity, caravans and cargo cogs you can follow down the roads, tolls, famines, and the occasional beached whale
- **A dynasty** — named notables who marry, scheme, and die; successions, regencies, usurpers, family trees you can browse; great houses with temperaments, grudges, and civil wars fought by armies that camp outside the walls
- **The fates** — weather and floods, fires with real flames, plague waves, bandit kings, comets and eclipses, wolf-winters, white harts, and one dragon
- **Towns that change** — houses rise with prosperity and rot with decline, forests are cleared into farmland, new villages are chartered, windmills and abbeys and wayside shrines appear over the decades
- **An auto-director and Watch mode** — leave it alone and a camera drifts to whatever story is breaking, with a caption card; press `C` and the realm narrates itself
- **The Chronicle** — every event inked with its date, filterable, exportable, with a Charts view of population, treasury, and prosperity across the centuries
- **Acts of God** — a board of interventions: plague, fire, earthquake, dragon-waking, assassination, contested successions, or the charter of a new village exactly where you click

## Controls

| Desktop | Touch |
|---|---|
| drag to orbit | one finger orbits |
| wheel / trackpad scroll or pinch to zoom | pinch zooms |
| right-drag to pan | two fingers pan |
| double-click to travel | double-tap travels |
| click to inspect | tap inspects |

`space` pause · `1–5` speed · `T` territories · `C` watch mode · `G` director · `L` labels · `H` hide the court · `Esc` release

## Running locally

Open `index.html` in a browser, or:

```
node server.js
```

and visit `http://localhost:8544`. The only network dependency is three.js r128 from a CDN.

## How it's built

Vanilla JavaScript and three.js r128, ~7,000 lines in one file. Four seeded RNG streams (worldgen, detail, ambient, history) keep generation and simulation deterministic and independent — the same seed always yields the same world and the same history, at any simulation speed. Rendering is triangle-soup geometry merged per settlement with one patched Lambert material carrying seasons, snow, drought, and window-glow, instanced trees, and a canvas-texture territory overlay: the whole realm draws in well under 100 draw calls at 60 fps.
