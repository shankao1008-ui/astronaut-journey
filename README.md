# An Astronaut's Solar System Walk

**▶ Live demo: https://shankao1008-ui.github.io/astronaut-journey/**

A first-person, brick-and-mosaic style walking tour of the Solar System and five exoplanets. You look out through a round helmet visor, see your own gloves and boots, and walk across the surface of real worlds while giant planets hang in the sky. Everything is built from data: relative sizes, axial tilts, gravity, temperatures and the angular size of each body in the sky follow NASA and ESA figures.

Built as a single HTML file with [Three.js](https://threejs.org/). Interface in English and Traditional Chinese (繁體中文) with a language switch in the top-left corner.

## The journey (18 stops)

| # | Stop | Where you stand |
|---|------|-----------------|
| 0 | Earth | Launch pad, brick rocket and tower |
| 1 | Dudu Planet | A fictional bonus world with photo boards and a photo moon |
| 2 | Moon | Lunar surface, lunar module and flag, Earth in a black sky |
| 3 | Venus | Lava plain in thick orange haze, Venera lander |
| 4 | Mercury | Cratered ground with long scarps, oversized Sun |
| 5 | Sun | Heat-shielded observation deck (no solid surface) |
| 6 | Mars | Red plain, rover, dust devil, Olympus Mons on the horizon |
| 7 | Jupiter → Europa | Cracked ice, Jupiter and its red spot overhead |
| 8 | Saturn → Enceladus | Ice geysers, Saturn with its rings seen edge-on |
| 9 | Uranus → Miranda | Patchwork ice and the Verona Rupes cliff, Uranus on its side |
| 10 | Neptune → Triton | Cantaloupe terrain and dark nitrogen plumes |
| 11 | Pluto | The heart-shaped nitrogen plain, Charon fixed in the sky |
| 12 | Proxima Centauri b | Estimated rocky surface under a red flare star |
| 13 | TRAPPIST-1 e | Estimated rocky surface, neighbouring planets in the sky |
| 14 | 51 Pegasi b | Observation deck above a hot Jupiter |
| 15 | Kepler-452 b | Estimated rocky surface, heavier gravity |
| 16 | SWEEPS-11 | Observation deck above a hot Jupiter near the Galactic core |
| 17 | Milky Way | Finale: the galaxy seen from the ship's window |

The Sun and the gas giants have no surface to stand on, so the Sun is visited from a deck and the giants from their moons.

## Features

- First-person view with helmet frame, animated gloves and boots, head bob and footprints
- Gravity that matters: jumps on Enceladus and Miranda last seconds; Kepler-452 b feels heavy
- Sky objects drawn as pixel mosaics at their true angular diameter (objects under 1.5° are enlarged to 1.5° for visibility)
- First-person ship transitions: boarding, takeoff, warp with an AU / light-year counter, approach and landing
- A brick companion astronaut who walks ahead and waves
- Touch and keyboard controls, portrait and landscape layouts, reduced-motion support, automatic quality scaling

## Controls

- After landing you keep walking forward. Drag the view to look around and steer.
- Press `Enter` to jump with the local gravity. **Info** reopens the fact card.
- Keyboard: hold `S` / `↓` to pause, `A` `D` / `←` `→` turn, `Q` `E` look up/down, `Enter` or `Space` jump, `N` / `P` next / previous stop.
- Add `?lang=en` or `?lang=zh` to the URL to force a language, `?station=N` to start at a stop.

## Files

- `index.html` — v2, the first-person walking version described above
- `classic.html` — v1, a third-person canvas animation of the same route (16 stops, no dependencies)

## Data and honesty

Data from NASA Solar System Exploration, NASA/JPL Horizons, NASA Exoplanet Archive and ESA Gaia. Scenes are brick-style illustrations with simplified terrain; distances are compressed; there are no real images of exoplanets, so their surfaces are inferred from size, temperature and host star. Generic brick style, unaffiliated with any brick brand.

---

## 太空人的太陽系之旅(繁體中文)

第一人稱、積木馬賽克風格的太陽系步行之旅:透過圓形頭盔窗看出去,看見自己的手套與靴子,在真實天體的地表上行走,巨大的行星掛在天上。太陽與氣態巨行星沒有固態地表,因此太陽站改在隔熱觀景艙,木星、土星、天王星、海王星則降落在它們的衛星。

單一 HTML 檔(使用 Three.js)。左上角可切換繁體中文 / English。`index.html` 為步行版,`classic.html` 為第三人稱經典版。降落後太空人會自動一直往前走,拖曳畫面環顧四周並改變方向;按 Enter 依該天體重力跳躍。

資料出處:NASA Solar System Exploration、NASA/JPL Horizons、NASA Exoplanet Archive、ESA Gaia。場景為積木風格示意圖,距離已縮短,天空中天體大小依真實角直徑估算;系外行星尚無實拍影像,地表外觀為推估。
