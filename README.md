# The Sector

A living, procedurally generated 15 × 15 km section of the Eastern Front, circa 1943–44.

This is a fork of [emollick/annals-kingdom](https://github.com/emollick/annals-kingdom). It keeps the original project's strongest ideas—a world in one HTML file, deterministic URL seeds, an autonomous simulation, a watch mode, and a chronicle—but replaces the medieval kingdom with a battalion-scale tactical staff exercise.

The scenario is intentionally synthetic. It creates period-shaped situations rather than claiming to reconstruct a named battle.

## Run it

Open index.html directly, or run:

~~~sh
node server.js
~~~

Then visit [http://localhost:8544](http://localhost:8544).

The only network dependency is Three.js r128 from a CDN. A URL seed recreates the same terrain, weather, OOB, actual strengths, and deployment:

~~~text
http://localhost:8544/#s=first-morning
~~~

## What is implemented

- A deterministic 15,000 × 15,000 metre terrain model with a 1 km staff grid
- Seeded lowland relief, river and bridge, roads, villages, woods, fieldworks, wire, and minefield belts
- Three period vignettes: autumn 1943 mud, winter 1943–44, and a summer 1944 breakthrough
- A depleted German 1944-type grenadier Kampfgruppe with two grenadier battalions, mobile reserve, assault guns, antitank guns, pioneers, and divisional artillery
- A reinforced Soviet rifle regiment with three rifle battalions, an SMG company, sappers, antitank guns, attached T-34s/SU-76s, and divisional artillery
- Per-unit paper and actual strength, morale, fatigue, suppression, ammunition, fuel, armor, antitank ability, and entrenchment
- Terrain- and weather-sensitive movement, local combat, artillery support, spotting, retreat, resupply, objective capture, and basic tactical AI
- Umpire, Soviet, and German fog-of-war views
- Selectable unit counters, inspectable TOE summaries, hold/dig/advance orders, watch mode, and a seeded combat journal
- A small test API at window.FRONT with stats(), step(), seed(), select(), and validate()

## Is the premise plausible?

Yes—with the right abstraction.

A 15 × 15 km map is too small for honest division-level maneuver and too large for individual squads. Battalion maneuver units, with specialist companies and artillery groups aggregated, are the useful middle. The map can plausibly show:

- most of a reinforced Soviet rifle regiment attacking on the central 4–6 km;
- one depleted German regiment's main line, depth positions, and local reserve;
- divisional artillery represented by firing groups rather than every battery position;
- neighboring formations and corps assets as off-map pressure or availability modifiers.

The force ratio is deliberately asymmetric but not fantastical. German formations on the Eastern Front were frequently well below establishment, while Soviet rifle formations also operated below their paper strengths and concentrated artillery, engineers, and armor at chosen breakthrough sectors.

| Model choice | Historical anchor | Game treatment |
|---|---|---|
| German 1944-type infantry regiment | The 1944 infantry division used three regiments of two battalions and had an authorized strength around 12,500 | Two depleted grenadier battalions plus regimental and divisional attachments |
| Soviet rifle regiment | A 1944–45 rifle regiment retained three rifle battalions plus regimental support elements | Three under-strength rifle battalions; supports are separate only when tactically interesting |
| Narrow Soviet main effort | Late-war Soviet divisions could be concentrated on a few kilometres in a breakthrough | The active fight occupies the centre of a wider 15 km context map |
| Thin German defense | Army Group Centre averaged roughly 14 km per division before Bagration, with density varying sharply by sector | One regimental sector is stretched across the map; depth and reserves matter |
| Strength vs. establishment | Both armies often fought well below paper TOE | Every seed applies deterministic under-strength factors and limited stocks |

Useful historical anchors:

- [U.S. Army, Handbook on German Military Forces, organization chapter](https://www.ibiblio.org/hyperwar/Germany/HB/HB-2.html)
- [U.S. Military Academy history: 1944 German infantry divisions](https://www.ibiblio.org/hyperwar/USA/USMA/WEurope1/WEurope1-2.html)
- [U.S. Command and General Staff College: Organization of a Russian Rifle Division, 1944–45](https://cgsc.contentdm.oclc.org/digital/api/collection/p15040coll6/id/5461/download)
- [U.S. Army Combat Studies Institute: Tactical Responses to Concentrated Artillery](https://www.armyupress.army.mil/Portals/7/combat-studies-institute/csi-books/tactical.pdf)
- [German History in Documents and Images: Eastern Front in April 1944](https://germanhistorydocs.org/en/nazi-germany-1933-1945/europe-in-april-1944)

The largest remaining historical abstraction is artillery. A Bagration breakthrough could involve extraordinary artillery densities far beyond what physically appears on this map. The current artillery counters therefore represent allocated firing groups and ammunition access, not literal organic strength.

## Design principles

1. Plausible, not falsely precise. Generated designations and strengths create a credible composite; they do not invent a fake historical record.
2. Actual strength matters more than paper TOE. A battalion's weapons, cohesion, and supply state are as important as its headcount.
3. The map is an umpire's model. Counters are oversized, forests are representative, and roads/villages are spatially meaningful rather than cartographically exact.
4. Logistics should produce stories. Mud, ammunition, fuel, fatigue, and bridges create decisions instead of acting as decorative modifiers.
5. The chronicle explains causality. Important changes become readable reports rather than hidden arithmetic.

## Fun next ideas

| Idea | Why it is fun | Implementation sketch |
|---|---|---|
| Fire-plan editor | The most distinctive late-war Soviet decision was often the preparation, not a click-to-shoot artillery spell | Add a pre-battle timeline with concentrations, smoke, ammunition allotments, lift times, and uncertainty from bad observation |
| Wire and radio command net | A perfect order arriving too late is a better story than instant omniscience | Build HQ-to-unit links; movement cuts field telephone lines, radio units risk interception, and orders enter a delayed queue |
| Counterbattery duel | Firing artillery should reveal itself and invite consequences | Track muzzle reports, sound-ranging estimates, displacement time, and ammunition vehicles; show probability ellipses instead of exact locations |
| Engineer drama at the bridge | One crossing can organize the whole generated battle | Give the bridge damage states, demolition circuits, mine lanes, assault boats, ferries, and a pontoon construction task with visible progress |
| Reconnaissance before the clock starts | Information becomes a resource rather than a view toggle | Let players assign patrols, prisoners, aerial photos, and reconnaissance-in-force; each reveals different layers at a cost in surprise |
| Deception and maskirovka | The generated map supports bluffing | Add dummy gun flashes, false radio nets, camouflage discipline, and decoy assembly areas that affect the AI's reserve commitment |
| Named micro-stories | The parent project's human chronicle is worth preserving | Generate a few commanders, runners, gun crews, and village civilians; surface them only when a system event makes their story relevant |
| Linked 72-hour campaign | Holding a road at noon should matter tomorrow | Persist surviving units, vehicle recovery, ammunition, bridge condition, displacement of civilians, and the frontline into the next generated sector |
| After-action map | Watching the battle is good; understanding it is better | Sample positions, losses, spotting, orders, and fire missions, then export a time scrubber and annotated SVG/PDF staff map |

The best next implementation slice is the fire-plan editor plus command delay. Together they make the simulation feel specifically Eastern Front 1943–44 rather than like a generic real-time strategy game.

## Controls

| Input | Action |
|---|---|
| Drag | Pan the staff map |
| Right-drag | Rotate and tilt |
| Mouse wheel | Zoom |
| Click a counter or OOB row | Inspect the unit |
| Double-click ground | Order the selected unit to advance |
| Space | Pause/resume |
| 1 / 2 / 3 | Set 1× / 6× / 30× |
| F | Cycle umpire/Soviet/German view |
| G | Toggle kilometre grid |

## Scope limits

- This is a game prototype, not a professional combat model.
- Loss calculations are calibrated for readable multi-hour stories, not prediction.
- Generated place and formation names may resemble real ones by coincidence.
- No political symbols are used; side colors and neutral military symbology carry the presentation.
- Civilian suffering, atrocities, and the political context of the war are not simulated. Their omission should not be mistaken for historical unimportance.

## License and attribution

MIT, following the upstream project. See [LICENSE](LICENSE).

The original concept, single-file architecture, seeded-world approach, and project lineage belong to [emollick/annals-kingdom](https://github.com/emollick/annals-kingdom). This fork is a substantial setting and simulation rewrite.
