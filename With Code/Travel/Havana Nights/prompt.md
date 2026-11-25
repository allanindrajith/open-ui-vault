# Noche Viva — recreation prompt

Build a single full-screen landing hero called **Noche Viva**: a pastel Caribbean colonial street hidden behind one flat cream apartment facade. On load the facade splits down its centre seam and its two blocks slide off left and right; through the widening gap the street appears as eight vertical pieces that start packed in the middle of the screen (outer pieces in front, so the big BODEGA and PALADAR signs show first) and fan outward to their true places. Then, driven only by the visitor's scroll, the street fades from daylight to neon night while the camera seems to tilt down: every column of the street rises along one smooth curve (edges a lot, centre barely), and a dark stone ledge with the title NOCHE VIVA rises in from below. Scrolling back up reverses it. Match everything below exactly.

## 1. Assets (use these exact URLs)

Every image goes through the Higgsfield image proxy: `https://images.higgs.ai/?default=1&output=webp&url=<URL-encoded source URL>&q=85&w=<width>`. The proxy sends `Access-Control-Allow-Origin: *`, so load images with `crossOrigin = "anonymous"` (they are uploaded as WebGL textures).

| Name | Source URL (encode it into the proxy) | Native size | Role |
|---|---|---|---|
| Day street | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261125_123040_f513c6f7-07d8-410f-a6de-4545268d7e1f.png | 2048 × 1360 | Daylight street, straight-on one-point perspective, vanishing point at the exact centre. Correctly spelled vintage neon blade signs (unlit): BODEGA (16–20 % of the width, left), LA HABANERA (horizontal, 20–32 %), SALSA (26–30 %), CINE, CAFÉ, BAR, TABACOS (37–45 %), BAR, HOTEL, PAN (57–62 %), RUMBA (70–75 %), PALADAR (81–86 %, right). |
| Night street | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261125_123132_feb5ca9b-fc48-4930-80a0-a8f2fb824330.png | 2496 × 1664 | The same street at night, every sign lit in burnt-orange neon, slate-blue sky. Aligned with the day image except a measured 2.5 px (of 1600) shift to the right over the left and centre (corrected in the shader, section 6.3). |
| Facade | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261125_063353_1267b8e6-3e85-4dde-b424-1647f0a4de2e.png | 2496 × 1664 | One flat cream colonial apartment block with orange and blue-grey shutters on a plain sand background (#EBE1D7). Building spans 30.7–70.0 % of the width; its vertical seam is at exactly 50 %. |
| Ledge band | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261125_063406_591aa9e3-ad36-4e74-9aa7-01de368920f9.png | 3120 × 1328 | Rough grey stone ledge across the top ~7 %, near-black underground concrete wall with a few small warm lights below. No text. |

Width to request: the smallest of 1280 / 1600 / 1920 / 2496 that is at least `cssWidth × min(devicePixelRatio, 2)`, capped at 2048 for both street images. Street `cssWidth = max(viewportW, viewportH × 1.26 × 1.506)`; facade `max(viewportW, viewportH × 1.5)`; band `viewportW`.

Fonts (self-hosted woff2, `font-display: swap`): **Black Ops One** 400 — https://fonts.gstatic.com/s/blackopsone/v21/qWcsB6-ypo7xBdr6Xshe96H3aDvbtw.woff2 (title; fallback Impact) and **Montserrat** variable 400–500 — https://fonts.gstatic.com/s/montserrat/v31/JTUSjIg1_i6t8kCHKm459Wlhyw.woff2 (nav; fallback system-ui).

## 2. Head, tokens, structure
- Title "Noche Viva — Havana after dark"; meta description "A colonial street that splits open, slides out building by building and falls into neon night."; viewport `width=device-width, initial-scale=1, viewport-fit=cover`; preconnect to images.higgs.ai.
- Tokens: `--stage #ebe1d7` (sand stage, hero background), `--ink #fff`, `--band #0b0b0c`. `html` background #0b0b0c, `overflow-x: clip`, scrollbar hidden. Never put `overflow` on `body` (it breaks the sticky hero).
- DOM: `div.track` (height 260svh, gives 160svh of scroll) → `main.hero` (sticky, top 0, 100 % × 100svh, overflow hidden, background `--stage`, `--u = 100vw/1440`). Inside, back to front: a hidden `.world` with two `<img>` plates (fallback only), `#layers` (absolute, inset 0, z 1; holds the WebGL canvas), `.band` (absolute, top 0, width 100 %, z 2, holds `h1.title`), the two facade halves `.half.l` / `.half.r` (z 3), then `nav` (z 5).

## 3. Geometry (computed on load and on every resize, debounced 120 ms)
W × H = hero size.
- **Street plate box**: height `ph = max(W / 1.506, 1.26 H)`, width `pw = 1.506 ph`, left `px = (W − pw)/2` (centred, overflowing both sides), top 0. 1.26 is the minimum that keeps the outer columns covered until the ledge arrives (max over the scroll of ledge-top + 0.32 × rise = 1.252).
- **Facade**: the facade image covers the hero (height H and width 1.5 H, or width W and height W/1.5 if that is wider). The two blocks rest with a thin sand gap between them: gap = 8.5/664 W on each side of the centre. Left half: a div pinned left, width `W/2 − gap`, overflow hidden, background `--stage`, containing the image at `left = W/2 − gap − fw/2`, vertically centred. Right half: pinned right, width `W/2 − gap`, image at `left = −fw/2`. Together they show one facade split at its seam with a 17/664-wide sand gap.
- **Ledge band**: height `(1 − 0.799 + 0.03) H`, band image `background-size: cover`, `background-position: 50% 0`, colour #0b0b0c. Its vertical position is animated (6.2).
- **Title** "NOCHE VIVA": Black Ops One, `font-size = min(0.118 H, 0.105 W)`, line-height 1, colour #f4f2ee, letter-spacing .01em, no wrap, `right: 3vw`, top `(1 − 0.799) H × 0.56 − fontSize/2` inside the band, text-shadow `0 0 18px rgba(255,255,255,.22), 0 0 2px rgba(255,255,255,.5)`.

## 4. The eight street pieces
Cut the street image into 8 full-height vertical pieces (sky and road included), in fractions of the image width:
A [0, .2045] (BODEGA) · B [.2045, .325] (LA HABANERA + SALSA) · C [.325, .35] · D [.35, .372] · E [.372, .628] (centre: CINE … HOTEL, never slides) · F [.628, .76] (RUMBA) · G [.76, .805] · H [.805, 1] (PALADAR).
These land on the reel's on-screen piece widths at a 1440 × 900 screen (A 0–12 %, B 12–20.5 %, C 20.5–27 %, D 27–35 %, E 35–65 %, F 65–80 %, G 80–86 %, H 86–100 %), nudged so no sign is cut. The two outer pieces carry an extra 9 % of mirrored image beyond the edge (mirror-sampled) so the frame side never opens.

**Start offset ("travel")**, recomputed on every resize: for each piece take the part that is on screen, its middle `c` (fraction of W); travel = 0.93 × |c − 0.5| × W toward the centre (0 for the piece that contains the centre). 0.93 is the reel's measured ratio (its outer pieces start .41 and .431 of the width toward the centre from .44 and .43 away). Outer pieces are drawn on top.

## 5. Rendering (WebGL2)
One canvas covering the hero (device pixels, dpr capped at 2). Both street images are textures (mipmapped, linear filtering, clamp). Each piece is a triangle strip of 48 columns × 2 rows spanning its image range. Vertex shader: final x `xf = px + u·pw`; `d = (xf − W/2)/W`; rise `r = min(0.32, 0.022 + 1.02·|d|^1.5)`; screen position `x = xf + pieceOffset`, `y = v·ph − pan·r·H`. Fragment shader: mirror u outside 0–1; day colour = day texture at (u, v); night colour = night texture at (u + 0.0016 × clamp((0.87 − u)/0.37, 0, 1), v) (the measured night offset); output `mix(day, night, nightAmount)`. Draw inner pieces first, outer last. The pieces are the picture from first frame to last — nothing is ever swapped in, so nothing can jump when they land. Without WebGL2, fall back to one div per piece holding a day canvas and a night canvas (night opacity animated), each piece rising by r at its own centre.

## 6. Motion
All curves are 30 fps lookup tables measured frame by frame from the reference reel (every frame tracked at 664 px wide, sub-pixel), read with linear interpolation; before the first value use the first, after the last use the last.

### 6.1 Load intro (time-driven, plays once)
Starts when all four images are loaded and decoded, the street is built and fonts are ready; T = seconds since then minus a **0.40 s hold**.
- **Facade blocks** move outward (left −x, right +x) by `s(T)·W`, s per 1/30 s: 0, 0, 1, 2, 3, 6, 10, 15, 22, 30, 40, 51, 64, 79, 95, 112, 129, 148, 166.5, 185, 203.5, 222, 240.5, 259, 277.5, 296, 314.5, 333, 351.5 (all ÷ 664) — slow start, then ~18.5/664 of the width per frame until they leave. Hide them when they have moved more than W/2 + 2 px.
- **Street window**: the street canvas is clipped (`clip-path: inset(0 X 0 X)`) to a window between the blocks, a sand margin of 8/664 W inside their inner edges: window half-width = (8.5/664 + s − 8/664)·W, X = max(0, W/2 − half-width). Remove the clip once the blocks are gone.
- **Pieces** slide out on one shared progress curve p per 1/30 s: 0, .005, .012, .019, .021, .03, .043, .061, .082, .109, .141, .176, .216, .261, .308, .358, .41, .464, .518, .573, .626, .678, .727, .773, .814, .852, .885, .915, .938, .958, .972, .983, .992, .996, .999, 1 (lands at T ≈ 1.17 s). Each piece reads it at `T + lead`, lead = max(0, 0.25 × (0.41 − travel/W)) s, so inner pieces lead by up to ~0.1 s. Offset = ±(1 − p)·travel (left pieces +x, right pieces −x).

### 6.2 Day → night, camera tilt, ledge (scroll-driven, reversible)
After the intro (T ≥ 1.17 s), scroll progress `sp = scrollY / (scrollHeight − innerHeight)` (0–1) drives a reel frame `k = 113 + sCur × 35`. `sCur` follows `sp` through a critically damped spring (ω = 9: `a = ω²(sp − sCur) − 2ω·v`, dt per frame capped at 50 ms, snap when error and velocity < 1e-4). Before the intro ends the target is 0. The page always opens at the top (`history.scrollRestoration = 'manual'`).
- **Night amount** from frame 113: 0, .02, .04, .05, .07, .09, .11, .14, .18, .21, .26, .30, .36, .41, .48, .54, .60, .67, .74, .79, .85, .91, .98, 1.
- **Pan** (the `pan` uniform) from frame 116: 0, .25, .5, 1.25, 2.5, 4.25, 6.5, 9.5, 13.25, 17.5, 22.5, 28.25, 34.25, 40.75, 47.75, 55.25, 62.75, 70.5, 78.5, 86.5, 94.25, 101.5, 108.5, 114.75, 120.75, 125.75, 130.25, 133.75, 136.5, 138.5, 139.5, 139.75 (÷ 139.75). Multiplied by the smooth rise r(d) per column: verticals stay vertical, the lines toward the vanishing point change angle, so it reads as the camera tilting, never as cut strips. The rise curve is fitted to the reel's measured piece rises (A .306, B .229, C .196, D .067, E .036, F .082, G .231, H .309 of the height).
- **Ledge band** top (fraction of H) from frame 137: 1.02, .978, .95, .921, .895, .871, .851, .834, .818, .807, .801, .799 — it enters from below only near the end of the tilt and eases to 79.9 %.
So: top of the page = day; mid-scroll = dusk with the street tilting; bottom = full neon night, the ledge and NOCHE VIVA in place.

### 6.3 Loop, fallbacks
One requestAnimationFrame loop, started on load, scroll and resize, stopped when the intro is done and the spring has settled. If the images are not ready after 9 s or fail, show the two plates directly with the night fade, a uniform 0.2 × pan rise and the ledge. `prefers-reduced-motion: reduce`: no intro and no spring — the street shows landed and scroll maps straight to night, tilt and ledge.

## 7. Navigation
Over everything at the top. Desktop: flex row, `space-between`, padding `max(14px, 34u)` top / `max(20px, 70u)` sides, Montserrat 400, `max(12px, 15u)`, letter-spacing .02em, white, text-shadow `0 1px 10px rgba(40,30,20,.28)`. Left group "plazas", "music", "rum"; right group "stays", "journal", "contact"; plain lowercase links (`href="#"`), gap `max(18px, 78u)`, 6 px vertical padding, line-height 1.2, min-width 24 px. Centre logo, absolutely centred at top 22u, 44u × 50u: a white SVG (viewBox 0 0 44 50) — arched doorway `M4 50V20C4 9 12 2 22 2s18 7 18 18v30h-7V20c0-6.6-4.9-11-11-11S11 13.4 11 20v30z`, sun disc (circle 22, 24, r 5.5), small door `M16 50V36h12v14h-4.5v-9.5h-3V50z`; drop-shadow `0 1px 6px rgba(40,30,20,.25)`; aria-label "Noche Viva home".

## 8. Responsive
- ≤ 820 px: logo first as its own centred row (30 × 34 px, 10 px below), then both link groups on one row (left / right), 12 px, gap 16 px, padding `14px + safe-area-inset-top` / 16 px.
- ≤ 360 px: 11 px, gap 12 px. Landscape phones (max-height 500 px): padding-top 10 px, 12 px, logo 28 × 32 at top 6 px, gap 18 px.
- The street box, piece offsets and window are recomputed for every shape, so the split, fan-out and tilt behave the same on phones, tablets and wide screens.
- Verified at 25 sizes from 320 × 568 to 2560 × 1440: nothing outside the viewport, no overlaps, text ≥ 11 px, tap targets ≥ 24 px, and the street covers everything above the ledge at every scroll position.

## 9. Acceptance checklist
- Frame 1: sand stage, the cream facade centred, seam exactly at the centre with a thin sand gap; nav faint over it.
- After 0.4 s the blocks part (slow, then fast) and leave by ~0.95 s; in the gap the outer pieces appear first, packed in the middle (BODEGA, PALADAR), then SALSA / LA HABANERA / RUMBA fan out; at ~1.2 s the street is whole, with no seams or jump.
- Scrolling down: the street fades to neon night while the camera seems to tilt down (edges rise most, buildings never cut or bent apart); the ledge rises in at the end and settles at 79.9 % with NOCHE VIVA. Scrolling up reverses it, softly.
- All sign lettering is correctly spelled in day and night.