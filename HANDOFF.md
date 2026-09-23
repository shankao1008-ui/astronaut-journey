# Project handoff — 太空人的太陽系之旅 / An Astronaut's Solar System Walk

This document gives a new collaborator (human or AI) everything needed to understand and change the project.

- Live site: https://shankao1008-ui.github.io/astronaut-journey/
- Repository: https://github.com/shankao1008-ui/astronaut-journey (public, GitHub Pages served from the `main` branch, root folder)
- Local working copy: `~/astronaut-journey/` (a git repository)
- Author: Pei-Shan Kao. Built 21 Sep 2026 with Claude from two Traditional Chinese spec documents (v1: third-person flight animation; v2: first-person brick-style walk, which replaced v1's route/UI/animation sections but kept its data tables).

## 1. What is in the package

| File | Purpose |
|------|---------|
| `index.html` | **v2, the main app.** First-person brick/mosaic walking tour. Single file: CSS + JS inline, Three.js r128 loaded from cdnjs. ~90 KB. |
| `classic.html` | v1. Third-person Canvas 2D animation of the same journey (16 stops). No external dependencies. ~63 KB. Not being developed further. |
| `dudu/1.jpg … 7.jpg` | The author's personal photos shown on "Dudu Planet". Resized to max 900 px. |
| `README.md` | Public introduction (English first, Chinese below). |
| `HANDOFF.md` | This file. |

## 2. The journey (18 stops, v2)

0 Earth (launch pad) · 1 Dudu Planet (fictional bonus, photo boards + photo moon) · 2 Moon · 3 Venus · 4 Mercury · 5 Sun (observation deck) · 6 Mars · 7 Jupiter → Europa · 8 Saturn → Enceladus · 9 Uranus → Miranda · 10 Neptune → Triton · 11 Pluto · 12 Proxima Centauri b · 13 TRAPPIST-1 e · 14 51 Pegasi b (deck) · 15 Kepler-452 b · 16 SWEEPS-11 (deck) · 17 Milky Way finale.

Design rules that came from the spec and should be kept:
- Only bodies you could really stand on are walked. Sun and hot Jupiters use the observation deck (`mode: 'deck'`); gas giants are visited from moons with the planet in the sky.
- Sky bodies are drawn at their **true angular diameter** (minimum 1.5° for visibility). Gravity, tilt, temperatures and sizes follow NASA/ESA figures.
- Honesty labels stay visible: brick-style illustration, distances compressed, exoplanet surfaces are estimates, Dudu Planet is fictional.
- Brick look uses a fixed 16-colour palette (`P` object). No brand names or logos.
- Interface is bilingual (繁體中文 / English) via the language bar; every user-visible string must exist in both.

## 3. How `index.html` is organised

Line numbers are approximate. Section headers in the code look like `/* ===== 名稱 */`.

| Section | What it does |
|---------|--------------|
| CSS (top) | Helmet frame (`#helmet` pseudo-elements), HUD layout (`#ui`), info card, buttons, language bar, finale labels, portrait media query, reduced-motion. |
| HTML body | `#gl` canvas, `#look` drag layer, `#fade`, `#phase` (landing banner), `#finale` labels, `#ui` HUD, `#about` dialog (has `.zh` and `.en` blocks), error box, then the Three.js script tag and the app script. |
| 基本工具 | RNG, value noise `vnoise`, palette `P`, helpers. |
| 馬賽克圓盤材質 | `mosaic()` builds a pixel-grid canvas texture; `PAINT` holds one painter function per body (earth, jupiter, saturn, saturnEdge, uranus, sun, redDwarf, hotJupiter, dudu, …). Painters return a palette colour for texture cell (u, v). |
| 站點資料 | `ST` array = the stations (Chinese text). Fields: `id, name, en, type, au|ly, site, mode ('walk'|'deck'|'finale'), g (gravity in g), tint, stats[[label,value]×4], fact, extra`. `EN` object = English overrides per id. `UI` = interface strings per language. `LANG`, `T()`, `L(st, field)`, `nameOf()` pick the language. `distText()` formats distances. |
| 地形定義 | `TERRAIN[id] = { h(x,z) → height (quantised to 0.5, `null` = no block), c(x,z,h) → colour }`. `craterField()` makes deterministic crater layouts. `TERRAIN.deck` is the observation-deck floor. |
| 渲染器與場景 | WebGL renderer at 1/3 resolution (`RES`) upscaled with `image-rendering: pixelated`; `rig` (player body, yaw) → `camera` (pitch); lights; groups `world`, `skyG`, `cockpit`. `box()`/`stud()` helpers build brick shapes. |
| 積木地形 | Two `InstancedMesh` sets (blocks + studs), `GRID × GRID` cells recentred around the player (`rebuildGrid`). `groundAt()`, footprints (`stamp()` darkens the cell). |
| 天空 | `makeStars()`, `skyBody(painter, angDeg, az, alt, opts)` places a mosaic sprite at the right angular size, `buildSky(st)` sets background, fog, light and sky bodies per station. |
| 照片 | `loadPhoto()` (via `<img>` so EXIF rotation is respected), `photoMoon()`, `photoBoard()` for Dudu Planet. |
| 道具 | Brick props: `flag, lander, rocket, rover, venera, mountain, deckProps`; `buildProps(st)` chooses per station and places plumes / dust devil. |
| 夥伴太空人與粒子 | Companion minifig (`companion`, `updateCompanion`), plume particles, dust devil, blinking deck lights. |
| 自己的手與腳 | Gloves attached to the camera, boots attached to the rig. |
| 玩家狀態與操作 | `player` state, auto-walk, gravity-based `jump()`, drag-to-look, keyboard, card auto-hide. |
| 過場 | Cockpit group in camera space, warp streaks (`updateStreaks`), departure/arrival mosaic discs (`FULL`, `DEPART` maps choose the painter), `go(to)`, `updateTransition()`, `arrive()`, `enterStation()`, landing banner `landingBanner()`. |
| 結尾 | Galaxy point cloud, projected HTML labels, `startFinale/updateFinale/leaveFinale`. |
| 介面 | `setCounter`, `updateUI` (writes all HUD text for the current language), `nextStation`, dots, `setLang`. |
| 主循環 / 啟動 | `frame()` with FPS-based quality step-down; URL parameters; start. |

## 4. Controls (current)

- After landing the astronaut walks forward automatically. Drag the view to look around and steer. Hold `S` / `↓` to pause.
- `Enter` or `Space` = jump (height and duration follow the station's gravity).
- `A` `D` / `←` `→` turn, `Q` `E` look up/down, `N` / `P` next / previous stop, `I` toggles the info card.
- Buttons: Info, ← Previous, Next stop →, progress dots (jump to any stop), language bar 繁中 / EN.
- Removed on request (do not re-add unless asked): Look up/down button, Jump button, Photo button, Walk button.

## 5. Transition behaviour

Next stop → fade to black → cockpit view (takeoff shake, departure body shrinking) → warp streaks with the log-interpolated AU / light-year counter → destination body growing as a mosaic disc → **landing sequence (walk stations only)** → new station, landing banner "已降落 X / Landed on X" for ~4 s, info card shows then hides after 7 s. No captions are shown during the flight (removed on request). Duration 6–10 s by distance; jumps via dots use a 4.5 s version.

**Landing sequence** (section 降落場景 in the code; state object `landing`, functions `startLanding / updateLanding / finishLanding`): at `f ≥ .9` of a flight to a `mode: 'walk'` station, the destination's `TERRAIN[id]` is pre-built into a light 20×20 preview grid (`landerBlocks/landerStuds`, `LGRID`) and shown from a third-person aerial camera. Phases over `landing.dur` (9 s, 4.5 s with reduced motion, ×0.7 for dot jumps): ① 0–.3 scan grid + moving scan line; ② .3–.5 green ✓ on the chosen site and red ✕ on the roughest samples; ③ .5–.75 the brick lander slides in and descends; ④ .75–.88 the four legs extend to different lengths read from `landingTerrain.h()` so the body stays level, thrust cuts, dust particles and a small camera shake on touchdown. The lander is the same brick rocket as on Earth's launch pad. After touchdown it stays standing on its levelled legs at the landing site when the view switches to first person, and the astronaut spawns about 7 blocks from it (`landing.justLanded` in `enterStation`), looking slightly up with the rocket ahead-left; the auto-walk path passes just clear of the legs. Then `arrive()` runs as before. Site choice: sample a 7×7 set of points 2 blocks apart, prefer a spot with height spread ≤ 1 block (levellable) and the largest spread among those, so levelling is visible. A "Skip landing" button (`#skipLanding`) jumps to the end. Deck stations (Sun, 51 Peg b, SWEEPS-11) never trigger it. Reduced motion skips ① and ② and removes the shake.

## 6. How to make common changes

- **Change text**: edit `ST` (Chinese) and `EN` (English) for station text; `UI.zh` / `UI.en` for interface strings; `.zh` / `.en` blocks in the HTML for footer and about panel.
- **Add a station**: add an object to `ST` at the right index, an `EN[id]` entry, `TERRAIN[id]`, a `case` in `buildSky` and `buildProps`, and `FULL[id]` (painter used for the approach disc; add a `PAINT` painter if needed). Update the "stops visited" stat in the finale entry and the README table.
- **Change a planet's look in the sky**: edit its `PAINT` painter or the `skyBody(...)` call in `buildSky` (angular size, azimuth, altitude).
- **Change terrain**: edit `TERRAIN[id].h` / `.c`. Heights should stay in 0.5 steps; keep the player's start area (near x=0.5, z=0.5) walkable.
- **Photos on Dudu Planet**: replace files in `dudu/`, and the list of positions/frame colours in `buildProps` case `'dudu'`. `photoMoon('dudu/1.jpg', …)` picks the moon photo.
- **Performance**: `GRID` (blocks per side, 76 desktop / 56 mobile) and `RES` (render scale). Auto step-down triggers below ~26 fps.

## 7. Testing and deploying

- Open `index.html` directly in a browser (needs internet for Three.js). URL parameters for testing: `?station=N`, `?lang=en|zh`, `?look=<pitch deg>`, `?yaw=<deg>`, `?fly=1` (auto-launch next leg), `?station=N&landk=0.62` (freeze the landing sequence at progress k for that station; `landk=1` plays the very end).
- Syntax check without a browser: extract the script and run `node --check`.
- Headless screenshots (macOS Chrome): add `--headless=new --use-angle=swiftshader --enable-unsafe-swiftshader --virtual-time-budget=5000 --screenshot=out.png`. Headless Chrome enforces a ~500 px minimum width, so test portrait layouts inside a 390 px iframe wrapper.
- Deploy: commit and push to `main`; GitHub Pages updates in about a minute.
  ```
  cd ~/astronaut-journey && git add -A && git commit -m "message" && git push
  ```

## 8. Known limitations and open ideas

- Requires network for the Three.js CDN; no offline fallback renderer.
- Shadows are not rendered (lighting is two-tone Lambert only).
- Sky objects are billboards; the planet does not rotate in the sky.
- Terrain is procedural noise plus simple features; no real elevation data.
- The personal photos on Dudu Planet are public because the site is public.
- Ideas not yet built: sound toggle, saving progress, real Canvas 2D fallback for weak devices, per-station ambient particles (snow on Enceladus, dust on Mars), more props.
