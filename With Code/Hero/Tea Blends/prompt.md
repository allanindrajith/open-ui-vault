# Emberleaf — Taste Autumn: recreation prompt

Give me the superdetailed exact prompt to recreate this page exactly as it is, including the correct asset URL, all elements, animations, and fonts.

Build a single-file, full-viewport, scroll-driven story page for a fictional tea house called **Emberleaf**. It is an editorial cut-out collage on flat warm colour fields. One fixed stage holds every layer; the document itself only provides scroll length. Scrolling moves the page through **8 fixed states** (a Figma "Smart Animate" chain). Every object keeps its identity and tweens its position, size, rotation and opacity between consecutive states. Nothing cuts; nothing is staggered; everything in a step moves together.

The story: autumn tea dunes → the six named ingredients → they fall into a glass funnel over the word BLENDS → a drop falls from the spout into a terracotta tea tin → the lid drops on → the three-tin product line rises on travertine plinths under the big wordmark.

---

## 1. Assets (all are direct Higgsfield results, never re-hosted)

Every image is loaded through the Higgsfield image proxy, which sends `Access-Control-Allow-Origin: *` (required, because three images are read back from canvas):

`https://images.higgs.ai/?default=1&output=webp&url=<URL-encoded result URL>&w=<width>&q=85`

| Key | Result URL | Proxy width | Content | How it is used |
|---|---|---|---|---|
| dunes | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261124_025933_fa37857a-0e54-406c-ae1a-9983cf44beaa.png | 2048 | Wide 16:9 photo of rolling autumn tea-shrub hills (amber, rust, olive) on a pure white sky, 2048×1152 | Drawn to a canvas after a runtime white key (section 5). The hill crest sits 413/1152 = 35.85 % down the image. |
| flowers | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261124_025937_de627bd0-51ca-4cd2-bb18-42078b8b6ae8.png | 2048 | Two clusters of orange marigolds on white, 2048 wide | Runtime white key, then two crops drawn into two canvases: left cluster box x 0, y 314, w 579, h 838; right cluster box x 1431, y 363, w 617, h 789 (pixels in the 2048-wide image). |
| ingr | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261124_030133_951f1e52-743a-4384-8e3b-b48d09875864.png | 2048 | Transparent sprite sheet 2048×1152 (Higgsfield background remover output): loose black tea, cardamom pods, cassia sticks, orange peel curl, dried rosebuds, amber resin, an amber drop | CSS background sprites (section 4) |
| tins | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261124_030106_c6bd583e-b8e6-415e-97de-f03e46986ba5.png | 2048 | Transparent sheet 2048×2048: three glazed round tea tins (cream, terracotta, apricot) with a gold leaf emblem, three matching ball lids, a short and a tall travertine plinth | CSS background sprites |
| funnel | https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/hf_20261124_025837_8a2d9b71-23b2-4369-b282-6a9fc7da56df.png | 1440 | Clear glass lab funnel on a flat apricot backdrop, 2880×2880 | Cropped to box x 622, y 184, w 1636, h 2578 (in 2880 px units, scaled by naturalWidth/2880), then the flat backdrop is keyed out at runtime (section 5) |

Sprite boxes (x, y, w, h in sheet pixels). Sheet sizes: ingr 2048×1152 and tins 2048×2048.

| Layer id | Sheet | x | y | w | h |
|---|---|---|---|---|---|
| i_tea | ingr | 57 | 144 | 437 | 343 |
| i_card | ingr | 563 | 158 | 420 | 290 |
| i_cassia | ingr | 1054 | 173 | 435 | 275 |
| i_peel | ingr | 1561 | 187 | 410 | 295 |
| i_rose | ingr | 61 | 655 | 409 | 346 |
| i_amber | ingr | 616 | 650 | 338 | 323 |
| drop | ingr | 1151 | 633 | 219 | 355 |
| tin_l | tins | 64 | 252 | 582 | 463 |
| tin_m | tins | 734 | 252 | 582 | 463 |
| tin_r | tins | 1403 | 251 | 580 | 463 |
| lid_l | tins | 188 | 902 | 334 | 329 |
| lid_m | tins | 855 | 903 | 333 | 328 |
| lid_r | tins | 1528 | 903 | 333 | 328 |
| p_m (short plinth) | tins | 87 | 1556 | 537 | 280 |
| p_l (tall plinth) | tins | 1449 | 1355 | 502 | 480 |
| p_r (tall plinth, mirrored) | tins | 1449 | 1355 | 502 | 480 |

Sprite CSS for a box in a sheet W×H: `background-size: (W/w·100)% (H/h·100)%` and `background-position: (x/(W−w)·100)% (y/(H−h)·100)%`, with no-repeat. Each sprite element's aspect ratio is h/w. Every sprite is a 400-frame-unit-wide base box (width 400·u px, height 400·u·aspect). Its displayed width comes from a transform scale of w/400. p_r is mirrored with a negative x scale.

Library: Lenis 1.1.13 (MIT), https://cdn.jsdelivr.net/npm/lenis@1.1.13/dist/lenis.min.js. It is inlined in the standalone file.

---

## 2. Font, colours, head

- **Font:** Marcellus Regular (400), latin subset, from Google Fonts (https://fonts.google.com/specimen/Marcellus). Self-hosted woff2 with `font-display: swap`, embedded as base64 in the standalone file. Every piece of text on the page uses Marcellus, with Georgia/serif as the fallback.
- **Colour tokens:**
  - `--ink #5a2e1f` (deep tea brown: nav, labels, wordmark)
  - `--head #c8653f` (terracotta headline)
  - `--sky1 #fbeee3`, `--sky2 #f0c9ae` (hero sky)
  - `--field #e9a17c` (apricot funnel field)
  - `--prod #f4d2bb` (peach product field)
  - `--white #fff8f0` (BLENDS lettering)
- **Head:** `<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">`, title "Emberleaf — Taste Autumn", preconnect to https://images.higgs.ai, and an empty data-URI favicon.
- **html:** background `--sky1`, `overscroll-behavior: none`, hidden scrollbar (`scrollbar-width: none` and `::-webkit-scrollbar{display:none}`). Text selection is off.

---

## 3. Scaling system and DOM

**Frame.** All geometry is written in "frame units" of a design frame, scaled into the viewport like `object-fit: contain`:
- Landscape (viewport aspect ≥ 0.9): DESK frame 1440×900.
- Portrait (aspect < 0.9): PORT frame 390×844.

**Scale and offsets:**
- `u = min(vw/W, vh/H)`
- `ox = (vw − W·u)/2`, `oy = (vh − H·u)/2`
- A frame point (x, y) lands at pixel (ox + x·u, oy + y·u).
- In PORT, horizontal positions spread out on wide phones: `spread = clamp((vw/u)/390·0.92, 1, 1.55)`, applied as `sx(x) = W/2 + (x − W/2)·spread`. It is 1 in DESK.

**Viewport edges in frame units:**
- `BELOW = (vh − oy)/u` (just below the viewport)
- `ABOVE = −oy/u` (just above it)
- These keep "off-screen" positions off-screen on any aspect ratio.

**Scroll track.** An empty `#track` div (1 px wide, height `7·stepPx + vh`) creates the scroll length. `stepPx = max(320, innerHeight)`, recomputed only when the width changes, so mobile address-bar resizes do not shift the steps.

**Stage.** `#stage` is `position: fixed; inset: 0; overflow: hidden; pointer-events: none`. Every layer is absolutely positioned at 0,0 and placed only with `translate3d / rotate / scale` (`will-change: transform, opacity`). Layer order, back to front:

1. `#bgSky`: linear-gradient(180deg, #fbeee3 0 %, #f8e2d1 45 %, #f0c9ae 100 %)
2. `#bgField`: flat #e9a17c, opacity animated
3. `#bgProd`: flat #f4d2bb, opacity animated
4. z 3:
   - `h1#headline`: two block lines "TASTE" / "AUTUMN", colour #c8653f, line-height .86, letter-spacing .01em, opacity .92, transform-origin 0 0
   - `#blends`: "BLENDS", colour #fff8f0, centred
   - `#mark`: "Emberleaf", colour #5a2e1f
5. z 4: `canvas#dunes`, horizontally centred (`left: 50%`, negative margin-left of half its width)
6. z 5:
   - ingredient sprites i_rose, i_tea, i_peel, i_cassia, i_amber, i_card
   - labels (letter-spacing .06em, ink) with this text:
     - t_rose "ROSE"
     - t_tea "BLACK TEA"
     - t_peel "PEEL"
     - t_cassia "CASSIA"
     - t_amber "RESINS"
     - t_card "CARDAMOM"
7. z 6: `#drop`
8. z 7: `#funnel`, containing a canvas that fills it
9. z 8: p_l, p_r. z 9: tin_l, tin_r, lid_l, lid_r. z 10: p_m. z 11: tin_m, lid_m.
10. z 12: canvases `#fl_l` and `#fl_r` (marigold corners)

**Nav.** `position: fixed`, top, z 40, a flex row with space-between:
- Brand link "Emberleaf" (href "/") on the left.
- A list of four links on the right: BLENDS, HARVESTS, SHOPPING, STORES (each href "/", letter-spacing .08em, ink colour, min-height 28 px; the brand has min-height 30 px).
- Focus-visible style: 1 px ink outline, 3 px offset.

| Nav value | DESK (n = min(vw/1440, vh/900)) | PORT |
|---|---|---|
| side padding | max(16, 78·vw/1440) px | max(16, vw·0.055) px |
| top padding | max(12, 30·n) px | max(14, 22·u) px |
| brand size | max(18, 30·n) px | max(18, 22·u) px |
| link size | max(11, 14·n) px | max(11, 11·u) px |
| link gap | max(14, 40·n) px | 8 px |

In portrait (max-aspect-ratio 9/10) the nav wraps: the brand sits on row one and the links spread across row two (space-between, row-gap 4 px).

**Type sizes (frame units × u):**
- headline 148 (DESK) / 72 (PORT)
- BLENDS 252 / 84
- wordmark 80 / 54
- labels max(11 px, 22·u) / max(11 px, 14·u)

**Dunes canvas size:**
- `hillPx = oy + hill·u`, with hill 380 (DESK) or 338 (PORT).
- Height `H = max(vw·0.5625, W·u·0.5625, (vh − hillPx)/(1 − 0.3585) + 2)`; in PORT also at least `(vh − hillPx)/(1 − 0.3585)·1.04`.
- Width H/0.5625.
- Its resting top is `hillPx − 0.3585·H`, so the hill crest lands on the hill line and the hills always reach the bottom edge.

**Marigold corners:**
- Width `flW·u` (flW 300 DESK, 130 PORT); height from each crop's aspect ratio.
- Left cluster at x = −0.06·width; right cluster at x = vw − 0.94·width.
- Both at y = vh − 0.86·height, so they bleed off the bottom corners.

---

## 4. The 8 states (frame units; x, y are the centre unless noted; w = displayed width; r = degrees)

**Shared:**
- `down = BELOW − dunesTopFrame + 40` for states ≥ 1. The dunes, both marigold canvases (`BELOW + 40`) and the headline translate down by this together, so the word stays behind the hill line as it leaves.
- `bgField` opacity is 1 in states 2–4. `bgProd` opacity is 1 in states 5–7.

### State 0: hero
- **Headline:** top-left at x 92, y 224 (DESK) or x 22, y 272 (PORT). Its lower half hides behind the dunes (word-behind-subject).
- **Ingredients tucked into the hills** (x, y, w, r):

| Layer | DESK | PORT |
|---|---|---|
| tea | 300, 560, 100, −8 | 70, 450, 62, −8 |
| card | 1035, 520, 92, 6 | 300, 430, 58, 6 |
| rose | 585, 628, 96, −4 | 180, 520, 62, −4 |
| peel | 1215, 612, 104, 10 | 330, 560, 64, 10 |
| cassia | 860, 575, 104, −6 | 120, 610, 66, −6 |
| amber | 430, 720, 78, 0 | 250, 640, 50, 0 |

- **Labels:** opacity 0, sitting 24 units below their grid spot.

### State 1: ingredient grid
- **Order and columns:** rose, tea, peel / cassia, amber, card.
  - DESK: 3 columns at x 292, 720, 1150; item rows at y 355 and 712; label rows at y 188 and 548. Labels sit above their item.
  - PORT: 2 columns at x 105, 285; three rows with items at y 232, 440, 648 and labels at y 140, 348, 556.
- **Widths:**

| Item | DESK | PORT |
|---|---|---|
| rose | 230 | 138 |
| tea | 240 | 146 |
| peel | 228 | 136 |
| cassia | 240 | 146 |
| amber | 180 | 110 |
| card | 236 | 142 |

- **Rotation and labels:** rotation goes to 0; labels reach opacity 1.
- **Hills:** the dunes, marigolds and headline are below the viewport.

### State 2: gather
- **Labels:** fade out and drift 14 units up.
- **Background:** the field fades to apricot.
- **Ingredients:** gather into one cluster around the centre (720, 175) for DESK or (195, 190) for PORT. Offsets (dx, dy, w, r):

| Layer | DESK | PORT |
|---|---|---|
| rose | −120, −20, 150, −10 | −70, −12, 92, −10 |
| tea | 10, −55, 165, 0 | 6, −34, 100, 0 |
| peel | 125, −5, 140, 12 | 74, −3, 86, 12 |
| cassia | −55, 55, 150, −14 | −34, 34, 92, −14 |
| amber | 75, 58, 108, 8 | 46, 36, 66, 8 |
| card | 0, 25, 140, 4 | 0, 15, 86, 4 |

- **Funnel:** rim width funW 483 (DESK) or 330 (PORT); height = funW · 2578/1636. It rises from `BELOW + 60` (top edge) to a top edge at 660 (DESK) or 640 (PORT).
- **BLENDS:** waits below the viewport at y `max(1080 DESK / 960 PORT, BELOW + fs·0.7)`.

### State 3: funnel / BLENDS
- **Funnel:** top edge moves to 89 (DESK) or 150 (PORT), centred at x W/2.
- **Ingredients inside the glass** (x, y, w, r):

| Layer | DESK | PORT |
|---|---|---|
| rose | 636, 178, 160, −8 | 150, 200, 88, −8 |
| tea | 806, 170, 168, 4 | 240, 196, 94, 4 |
| cassia | 660, 262, 152, −16 | 160, 258, 84, −16 |
| card | 790, 270, 148, 10 | 232, 262, 80, 10 |
| peel | 716, 350, 132, 6 | 185, 316, 74, 6 |
| amber | 724, 424, 96, 0 | 204, 368, 56, 0 |

- **BLENDS:** rises to centre y 600 (DESK) or 548 (PORT), behind the glass (the funnel stem crosses the letters).

### State 4: drip
- Funnel, ingredients and BLENDS all shift up by `shift4` (−560 DESK / −330 PORT).
- A drop appears at the spout: width dropW (34 / 22), centred at `funTop3 + funnelHeight + shift4 + dropHeight·0.5`.
- The drop is waiting in states ≤ 3: width dropW·0.35, opacity 0, centred at spout − 0.2·dropHeight.

### State 5: big tin catches the drop
- **Background:** changes to peach.
- **Leaving upward:**
  - funnel top edge at `funTop3 + shift5 − 0.4·funnelHeight` (shift5 −810 DESK / −610 PORT)
  - ingredients at y + shift5 − 200 with opacity 0
  - BLENDS at y3 + shift5 − 300 with opacity 0
- **Drop:** falls to `big.top + 0.2·dropHeight`, width dropW·0.8, and fades out as it enters the tin's mouth.
- **Middle tin:** rises from below the viewport (top at BELOW + 40) to a big close-up:
  - width 600 (DESK) or 330 (PORT), top at 480 (DESK) or 470 (PORT)
  - so its mouth is mid-screen and the body bleeds off the bottom
- **Middle lid:** waits just above the viewport (`ABOVE − lidHeight·0.9`), width = tin width·0.45.

### State 6: lid drops on
- **Middle lid:** drops onto the big tin; its bottom sits 5 % of the tin's height below the tin's top.
- **Side tins, lids and plinths:** rise to a "low" row, which is the final row pushed down by `rise + max(0, BELOW − H)` (rise 420 DESK / 300 PORT). Their tops peek in at the bottom edge.

### State 7: product line-up
- **Wordmark:** "Emberleaf" fades in centred at x W/2, y `max(166 DESK / 215 PORT, (navBottomPx + 6 − oy)/u + fs·0.62)`, so it never touches the nav. It starts 18 units higher at opacity 0.
- **Middle group:** the big middle tin and lid shrink into the middle of a three-tin row.

| Row value | DESK (left, mid, right) | PORT (left, mid, right) |
|---|---|---|
| Plinth centre x | 375, 720, 1065 | 76, 195, 314 (×spread) |
| Plinth top y | 600, 765, 600 | 620, 712, 620 |
| Plinth widths (tall, short, tall) | 340, 420, 340 | 148, 184, 148 |
| Tin widths | 290, 320, 290 | 120, 140, 120 |

- **Tins:** each tin's bottom sits 6 units below its plinth top.
- **Lids:** each lid is 0.45 of its tin's width, and its bottom sits 5 % of the tin's height below the tin's top.

**Off-screen positions:**
- Before state 6 the side products sit at a "gone" row: rise = `BELOW − min(plinthTop) + maxTinWidth·(tinAspect + 0.45·lidAspect) + 60`, so even the lids are hidden on tall screens.
- The middle plinth is gone until state 6 and low in state 6.

---

## 5. Runtime keys (canvas)

- **White key** (dunes and flowers):
  - Load with crossOrigin "anonymous" and draw to a canvas.
  - For each pixel, m = max(255−R, 255−G, 255−B), and alpha = clamp((m − 14)/46, 0, 1).
  - Where 0 < alpha < 1, un-mix the white: C = 255 − (255 − C)/alpha.
- **Funnel key** (flat apricot backdrop):
  - Average a 24×24 patch at (4, 4) as the backdrop colour B.
  - Crop the funnel box and compute dist = |pixel − B| (Euclidean RGB).
  - a = clamp((dist − 5)/70, 0, 1); pixels at 0 become transparent. Otherwise a = min(1, a·1.05 + 0.04), and un-premultiply: C = B + (C − B)/a.
  - The glass stays see-through and the bright rim highlights stay.

---

## 6. Motion and interaction (exact)

**Source setting.** Every step is Figma *On drag → Navigate to next frame → Smart Animate, Ease out, 1000 ms*. Ease out = `cubic-bezier(0, 0, 0.58, 1)`.

**Scroll-scrub.**
- Progress `P = scrollY / stepPx`, from 0 to 7.
- Each frame renders `lerp(state[floor(P)], state[floor(P)+1], P − floor(P))` for every property (x, y, w, r, o, dy), linearly. The layers follow the scroll or finger exactly, as in Figma's drag.

**Smooth wheel.** `new Lenis({ lerp: .1, smoothWheel: true, syncTouch: false })`, driven by the page's own requestAnimationFrame loop (`lenis.raf(now)`), so mouse wheels glide while touch uses native momentum scrolling.

**Settle on release** (Figma finishing the transition after the drag ends):
- **When it fires:**
  - Wheel: when Lenis is smoothing and no wheel event has arrived for 140 ms.
  - Touch and scrollbar: when the finger is up and the scroll has not moved for 140 ms.
- **Which step it finishes on:**
  - Moving down: go to the next state once more than 12 % into the step, otherwise back to the previous one.
  - Moving up: the mirror of that.
- **The settle tween:** `lenis.scrollTo(target, { duration: max(0.25 s, remainingFraction · 1.0 s), easing: cubic-bezier(0,0,.58,1) })`.
- **Interrupting:** a new wheel or touch interrupts the settle.

**Keys:**
- ArrowDown / PageDown / Space: next state. ArrowUp / PageUp: previous state. Home / End: first / last state.
- Each jump scrolls with duration max(350 ms, distance·1000 ms), capped at 1600 ms, using the same ease-out.

**Load.**
- Scroll restoration is manual, and the page starts at the top.
- The nav and headline are hidden until the fonts and the five images are ready (failsafe 2.5 s).
- Then the nav fades from opacity 0 / translateY(−10 px) and the headline fades from opacity 0 / translate 0 24 px (120 ms delay), each 1000 ms with the same ease-out.

**Resize.** Recompute the frame, rebuild all 8 states and keep the same progress P.

---

## 7. Reduced motion

With `prefers-reduced-motion: reduce`:
- Lenis is not created.
- Settles and key jumps are instant: they snap the scroll to the exact state.
- There is no load fade.
- The page remains a scroll-scrubbed story, with no autonomous movement.

---

## 8. Acceptance checklist

- **Scrolling:** a continuous trackpad scroll or a long phone swipe carries through several states. A small nudge springs back. The page scrolls to state 7 at the bottom and all the way back to state 0.
- **Hero:** the headline's lower half sits behind the hill crest; the marigolds bleed off both bottom corners.
- **Grid:** three columns on desktop, two columns × three rows on phones.
- **Funnel:** the glass is transparent (no apricot box). The BLENDS letters show through and behind the stem.
- **Off-screen:** no tin, lid or plinth is visible before state 6 on any aspect ratio.
- **Wordmark:** in state 7 it never overlaps the nav.
- **Sizes:** works from 320×568 phones up to 2560×1440, including landscape phones (DESK frame, contain-scaled).
- **Network:** the only requests are images.higgs.ai (plus the Lenis CDN when not inlined).