# Frostgate Kitchen — layered scroll-dive hero (exact recreation brief)

Build a single-page, full-screen, scroll-driven hero for a fictional winter mountain kitchen called **Frostgate Kitchen**. There is no headline, no button and no copy besides a thin centred navigation row: the illustrated scene *is* the hero. As the visitor scrolls, a pinned stage plays one continuous camera move built from 13 separately cut-out illustration layers that slide at different speeds (depth parallax): it starts on a snowy lilac monastery at the top of a stone staircase, an ice-rock arch rises from below and frames a frozen lake, then the arch rises further and zooms in until its own opening becomes the mouth of an ice cave, where a wooden gate stands on a small snowy island and pale violet prayer flags hang in front. Everything is driven by scroll progress (no autoplay, no timers except the load fade), smoothed by Lenis, and it plays back exactly in reverse when scrolling up.

Palette family: "Pristine lilac" — #E1DEDF, #C3C3CD, #AFB8CA, #9BA9C2, #8294B3 and ink #14182A.

---

## 1. Assets

Every layer is a full 16:9 PNG cut-out (2688 × 1520 px, transparent background unless noted) generated so that, stacked at offset 0 in one shared frame, they form the finished picture. Load each one through the Higgsfield image proxy as WebP at full width. The proxy URL pattern is:

https://images.higgs.ai/?default=1&output=webp&url=ENCODED_SOURCE&w=2688&q=85

where ENCODED_SOURCE is the URL-encoded source PNG below (encodeURIComponent of the whole https URL). All source PNGs live under https://d8j0ntlcm91z4.cloudfront.net/user_38xzZboKViGWJOttwIXH07lWA1P/ and every proxied URL was checked to return 200 image/webp.

| Key | What it shows | Source PNG (append to the folder above) | Ready-to-use proxied URL |
|---|---|---|---|
| sky | Opaque full-frame backdrop: pale lilac-blue sky fading to misty distant snow mountains along the horizon | hf_20261124_032051_b03be317-55bc-49c0-9643-583fc5604213.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032051_b03be317-55bc-49c0-9643-583fc5604213.png&w=2688&q=85 |
| side | The white walled side halls/wings of the monastery spanning the width at mid height (bottoms at ~66% of the frame) | hf_20261124_032150_6a19ddba-1bad-419f-9e29-dca0828c11eb.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032150_6a19ddba-1bad-419f-9e29-dca0828c11eb.png&w=2688&q=85 |
| stairs | Clean snowy stone staircase with stupa-topped balustrades and terraced snowy walls on both sides, NO people; soft fade to transparent above ~50% height; reaches the bottom edge | hf_20261124_050711_a8f1e754-6cb0-4763-b6a4-e7acd19f6d16.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_050711_a8f1e754-6cb0-4763-b6a4-e7acd19f6d16.png&w=2688&q=85 |
| hall | The main hall: lilac snow-covered tiled roof with gold finials, red-and-gold painted porch, dark door, three small robed monks at the door, its own top steps down to ~67% | hf_20261124_032131_a39a98b6-be3d-473c-a0a1-5ad2850557e7.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032131_a39a98b6-be3d-473c-a0a1-5ad2850557e7.png&w=2688&q=85 |
| people | Only the small robed monks on the staircase (navy, slate, wine red, tan), full bodies, transparent elsewhere; also contains a few figures higher up that are masked away | hf_20261124_050913_015b6505-badf-4834-b44e-dca4af03c966.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_050913_015b6505-badf-4834-b44e-dca4af03c966.png&w=2688&q=85 |
| larch | Two tall frosted larches hugging the left and right edges, full height | hf_20261124_032234_5b1a89ba-826e-4bdd-9416-0faf5b650f82.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032234_5b1a89ba-826e-4bdd-9416-0faf5b650f82.png&w=2688&q=85 |
| pav | Two stacked white pavilion towers left and right of the stairs | hf_20261124_032301_6b8aa813-522f-412c-b90d-0bfaf40c7590.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032301_6b8aa813-522f-412c-b90d-0bfaf40c7590.png&w=2688&q=85 |
| mid | Two clusters of snowy pines, mid depth, left and right | hf_20261124_032324_f9cb6fb0-9f9d-4f42-adcb-cfeae0e153da.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032324_f9cb6fb0-9f9d-4f42-adcb-cfeae0e153da.png&w=2688&q=85 |
| near | Big foreground frosted pine boughs framing both sides and the bottom corners | hf_20261124_032346_c94ad70e-0a0f-4a4e-99f8-39e0200da791.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032346_c94ad70e-0a0f-4a4e-99f8-39e0200da791.png&w=2688&q=85 |
| lake | Opaque full frame: frozen cracked lilac lake, misty mountains, a weathered wooden gate (torii-like) on a small snowy island in the centre with its long reflection | hf_20261124_032412_9f658303-9bca-4653-a7e8-7fcb3788f887.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032412_9f658303-9bca-4653-a7e8-7fcb3788f887.png&w=2688&q=85 |
| walls | Dark blue-violet glacier-rock cave walls on the left and right with a thin rocky ceiling, transparent mouth in the middle | hf_20261124_032423_d01ee5a9-9ec6-424c-93bb-9972ecf2f3cf.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032423_d01ee5a9-9ec6-424c-93bb-9972ecf2f3cf.png&w=2688&q=85 |
| arch | A natural ice-rock arch: frosted pines on its shoulders, dark blue rock band from ~44% to ~61% of the frame height across the middle, a wide transparent opening below 61%, two legs down to the bottom edge | hf_20261124_025152_784dec8b-ad51-476d-9e19-dbca11ca8d01.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_025152_784dec8b-ad51-476d-9e19-dbca11ca8d01.png&w=2688&q=85 |
| flags | Dozens of long narrow hanging prayer flags in violet, lilac and pale grey with frayed snowy ends, hanging from the top edge across the upper half | hf_20261124_032437_291e3d42-e769-437c-abcf-ecf2965a8feb.png | https://images.higgs.ai/?default=1&output=webp&url=https%3A%2F%2Fd8j0ntlcm91z4.cloudfront.net%2Fuser_38xzZboKViGWJOttwIXH07lWA1P%2Fhf_20261124_032437_291e3d42-e769-437c-abcf-ecf2965a8feb.png&w=2688&q=85 |

Every layer image is created as an img element with empty alt, decoding async, class "layer". The stairs image is used three times and the people image twice (depth bands, see §5), and the larch, mid and near images are each used twice (left half and right half), so the page holds 19 img elements.

## 2. Fonts and libraries

| Item | Value |
|---|---|
| Font | Jost, weight 500, normal style, latin subset. Google Fonts: https://fonts.googleapis.com/css2?family=Jost:wght@500&display=swap — the delivered file embeds the latin woff2 as a base64 data URI in an @font-face (font-display swap). Fallback stack: system-ui, sans-serif. |
| Smooth scroll | Lenis 1.1.13 from https://cdn.jsdelivr.net/npm/lenis@1.1.13/dist/lenis.min.js, options: lerp 0.085, wheelMultiplier 0.9, smoothWheel true. Do NOT rely on autoRaf (that version ignores it): drive it yourself by calling lenis.raf(now) inside your own requestAnimationFrame loop. |

## 3. Document, tokens and base styles

Title "Frostgate Kitchen", lang en, charset utf-8, viewport width=device-width, initial-scale=1.

| Token | Value |
|---|---|
| --ink | #14182A (nav text and logo stroke) |
| --lilac-1 | #E1DEDF |
| --lilac-2 | #C3C3CD (html background and the stage fill behind the scene) |
| --lilac-3 | #AFB8CA |

Global reset: every element box-sizing border-box, margin 0, padding 0. Body: background a fixed vertical gradient from --lilac-1 at 0% to --lilac-3 at 100%, font Jost, colour --ink, antialiased font smoothing.

## 4. DOM structure and layout

1. main.dive (id dive): position relative, height 420vh. This is the scroll track.
2. Inside it, div.stage: position sticky, top 0, height 100vh (100svh where supported), overflow hidden. The stage stays pinned while the track scrolls past.
3. Inside the stage, div.card (id card): absolute, inset 0, overflow hidden, background --lilac-2. It starts at opacity 0 and fades to 1 over 0.9 s ease-out when the root gets the class "ready" (it is full screen at every stage — it never shrinks into an inset card).
4. Inside the card, div.frame (id frame): the shared 16:9 picture frame. Absolute, left 50%, top 0, width = max(100%, 177.78vh) (use svh where supported), aspect-ratio 16/9, base transform translateX(-50%). So it always covers the viewport: on wide screens it is viewport-wide and taller than the viewport; on tall/phone screens it is viewport-tall and wider than the viewport (centred, sides cropped). It is top-anchored.
5. All layers are direct children of the frame in this paint order (later = in front): sky, side, stairs (base), hall, stairs band 2, people band 2, stairs band 3, people band 3, larch (two halves), pav, mid (two halves), near (two halves), then div.cave, then arch, then flags.
6. div.cave: absolute, inset 0. It contains div.cave-in (absolute, inset 0, transform-origin 50% 50%) which holds the lake layer, and after cave-in, directly in cave, the walls layer (so the walls do not zoom with the interior).
7. Every img.layer: absolute, left 0, top 0, width 100%, height 100%, display block, user-select none, no drag, pointer-events none, will-change transform. The arch image gets transform-origin 50% 100% (bottom centre).
8. nav.nav (aria-label "Main") is the last child of the card, above the scene (z-index 5).

### Navigation

A single centred row pinned near the bottom: absolute, left 0, right 0, bottom clamp(14px, 3.4vh, 34px), flex, items centred, justify centre, gap clamp(18px, 4.2vw, 64px). Order: BROTH (/broth), SMOKED (/smoked), BREADS (/breads), the logo (/, aria-label "Frostgate Kitchen home"), CELLAR (/cellar), SAUNA (/sauna), TEAM (/team). Links are plain text: font-size clamp(11px, 0.78vw, 12.5px), letter-spacing 0.34em, uppercase, colour --ink, no underline, padding 6px 0, no wrapping. A soft light halo keeps the dark labels legible over snow and ice: text-shadow 0 0 6px rgba(240,238,244,0.95), 0 0 14px rgba(225,222,236,0.9), 0 0 22px rgba(225,222,236,0.7); the logo svg gets filter drop-shadow(0 0 4px rgba(240,238,244,0.95)) drop-shadow(0 0 10px rgba(225,222,236,0.85)). No pills, no buttons, no underlines, no hover effects.

The logo is an inline SVG mountain line mark, viewBox 0 0 48 22, no fill, stroke currentColor, stroke-width 1.4, round joins and caps, three paths: a ridge "M1 21 L15 4 L24 14 L31 7 L47 21" and two small inner peaks "M10 21 L15 15 L19 21" and "M27 21 L31 16 L36 21". The logo link is display block, width clamp(36px, 3.1vw, 48px), padding 6px 0 with margin -6px 0 (so its tap target is ≥ 24 px without shifting the row); the svg is block, width 100%, auto height.

Nav entrance: every nav child starts at opacity 0 and translateY(10px). When the root gets "ready", each plays a 1 s rise to opacity 1 and no transform with easing cubic-bezier(0.22, 1, 0.36, 1), fill forwards, delay 0.25 s + index × 0.07 s, where the index runs 0–6 left to right (logo is 3).

## 5. The scroll choreography (the heart of the page)

### Progress

Each animation frame: p = (current Lenis scroll − dive.offsetTop) / (dive height − window.innerHeight), clamped 0–1. Then u = (p − 0.03) / 0.94 clamped 0–1, so there is a short still moment at the very top and bottom. Re-render only when p changed by more than 0.00001.

### Three keyframes on one smooth curve

Every animated value is defined at three keys — A (u = 0, the monastery), B (u = 0.42, the arch framing the lake) and C (u = 1, inside the cave) — and interpolated on ONE continuous curve: a monotone cubic Hermite spline through (0, A), (0.42, B), (1, C). The tangent is 0 at A and at C (ease in from rest, ease out to rest). The tangent at B is the weighted harmonic mean of the two segment slopes, d0 = (B − A)/0.42 and d1 = (C − B)/0.58: m = (0.42 + 0.58) / (0.42/d0 + 0.58/d1) when d0 and d1 have the same sign, otherwise 0. Each segment uses the standard Hermite basis with h = 0.42 or 0.58. Result: the move never stops at B, nothing overshoots, and every layer starts and stops at the same moments, only travelling different distances (that difference is the parallax).

### Layer keys (vertical offset in frame heights, applied as translate3d(dx%, y×100%, 0))

| Layer | y at A | y at B | y at C | Extra |
|---|---|---|---|---|
| sky | 0 | 0 | −0.12 | — |
| side | 0 | −0.15 | −0.60 | soft bottom fade 10 |
| stairs (base, full image) | 0 | −0.10 | −0.60 | — |
| hall | 0 | −0.10 | −0.60 | — |
| stairs band 2 | 0 | −0.15 | −0.63 | visible from 66% of its height down (top edge feathered over 1.2%); bottom fade 5, fade speed 40 |
| people band 2 | 0 | −0.15 | −0.63 | static mask: only the strip 66.8%–68% |
| stairs band 3 | 0 | −0.20 | −0.66 | visible from 72% down (feather 1.2%); bottom fade 7, fade speed 30 |
| people band 3 | 0 | −0.20 | −0.66 | static mask: visible from 67.5% (feathered to 68%) down |
| larch (2 halves) | 0 | −0.20 | −0.80 | halves pushed out ±7% of the frame width; bottom fade 30 |
| pav | 0 | −0.20 | −0.85 | bottom fade 14 |
| mid (2 halves) | 0 | −0.27 | −0.95 | halves pushed out ±5%; bottom fade 18 |
| near (2 halves) | 0 | −0.36 | −1.20 | halves pushed out ±6%; bottom fade 34 |
| flags | 0.14 | 0.14 | 0 | opacity ramps 0 → 1 linearly between u = 0.55 and u = 0.90; static top mask: transparent at 1%, opaque from 13% |

Tree halves: the larch, mid and near images are each placed twice. The left copy is clipped with clip-path inset(0 50% 0 0) and translated −dx×100% horizontally; the right copy is clipped with inset(0 0 0 50%) and translated +dx×100%. The two halves sit further apart than in the source image, so the side halls behind show between them.

Stair depth bands: the full stairs image stays behind the hall as the base. Two more copies of the same image are drawn in front of the hall: band 2 shows only from 66% of its height down, band 3 from 72% down, each with a 1.2% feathered top edge. The lower the band, the faster it rises, so each nearer band slides up over the one behind it (never apart, so no gap opens), and the joins run along the step lines. The people image is masked into the same bands and moves with them, so every monk rides in front on the steps they stand on and is never cut. The figures above 66% in the people image are masked out entirely, so the hall's own three door monks are the only ones at the door.

Soft bottom fade ("fade" in the table): for a layer that lifts, its cut bottom edge must never show as a hard line. Each frame, f = fade × min(1, −y × fadeSpeed) (fadeSpeed defaults to 6). Set the layer's mask-image (and -webkit-mask-image) to a linear-gradient to bottom: transparent until the band top when the layer is a band (top% transparent, top+1.2% opaque), opaque down to (100 − f)%, then transparent at 100%. At rest (y = 0) the mask is none (or just the band top), so the picture at A is untouched.

### The arch

| Value | A | B | C |
|---|---|---|---|
| translateY (frame heights) | 1.00 (fully below the frame) | 0.16 | −0.07 |
| scale (origin bottom centre) | 1 | 1 | 1.75 |

Transform: translate3d(0, y×100%, 0) scale(s). From A to B it rises from below until its rock top sits about 60% down and its opening is a smaller window onto the lake. From B to C it keeps rising and zooms 1.75× from its base, so the opening grows past the sides and becomes the cave mouth framing the gate, while the rock above it fills the top of the screen.

Arch bottom fade: once past B, with tC = (u − 0.42)/0.58 clamped, fb = 26 × clamp(tC / 0.35, 0, 1). The arch's mask-image is a linear-gradient to bottom, opaque to (100 − fb)% and transparent at 100%, so its lifting legs dissolve into the cave walls instead of showing a cut line. Before B, no mask.

### The cave

The cave interior (cave-in) is keyed too: translateY 0.22 → 0.20 → 0 frame heights and scale 0.8 → 0.8 → 1 (origin the frame centre) at A/B/C, so the lake and gate rise into the middle of the screen and zoom in slightly as the arch zooms past them. The walls layer stays pinned inside the cave.

The whole cave group is only ever seen through the arch's opening, using a soft moving mask rather than a clip. With the arch's current translate ay and scale as, the opening line in frame coordinates is seal = (1 + (0.6 − 1) × as + ay) × 100%, where 0.6 is the opening's top line inside the arch image. The feather is 11 × as percent. The cave's mask-image is a linear-gradient to bottom: transparent at (seal − feather)%, opaque at seal%. So the cave fades in under the arch's rock band and is hidden everywhere above it. Set the cave to visibility hidden when seal − feather is at least 100%, and visible otherwise.

### The frame pan

On screens where the 16:9 frame is taller than the viewport (spare = viewport height − frame height, negative), the frame also pans up by spare × 0.6 × K(u), where K is the same spline through 0, 1, 1. That is translate3d(−50%, that many px, 0). It brings the lake into view by B and holds there.

### Reading of the move

At the top you see the whole monastery: the hall at the top of the stairs with its three door monks, the side halls showing between the pushed-out trees, pavilions, and monks on the stairs. Scrolling, everything lifts together. The near pines lift fastest, then the mid pines, pavilions and larches, then the side halls, with the hall and upper stairs lifting least. The front stair bands slide up over the upper ones carrying their monks, while the ice arch rises fastest from below. At B the arch frames a window onto the frozen lake and the gate. Continuing, the monastery leaves through the top and the arch rises and zooms until it becomes the cave around the gate. The lake rises into the middle and the violet flags fade in hanging in front of the rock. Scrolling up reverses everything exactly.

## 6. Load

All 19 images are created up front. When every image's decode() promise has settled (or after a 5 s failsafe, whichever comes first), add the class "ready" to the html element: the card fades in (0.9 s) and the nav items rise in, staggered. Render once at p = 0 before the first frame, then run the rAF loop.

## 7. Responsive behaviour

The frame logic covers every aspect ratio: wide screens see the full width (and pan at B), and tall screens see the centre column of the scene at full height. Every motion value is in frame-relative units, so the choreography is identical at every size.

At 820 px wide and below, the nav wraps:
- flex-wrap on, row gap 6px, column gap clamp(14px, 5vw, 26px), side padding 16px, bottom max(14px, env(safe-area-inset-bottom))
- the logo goes first on its own row (order −1, flex-basis 100%, centred, no negative margin, svg 40px wide)
- links get letter-spacing 0.24em and padding 8px 0

In short landscape (max-height 480px and landscape), the nav goes back to one row:
- no wrap, gap clamp(10px, 2.6vw, 22px), bottom 10px
- logo in normal order, svg 34px
- links letter-spacing 0.2em, padding 6px 0

Text never goes below 11 px, every tap target is at least 24 px, and nothing scrolls horizontally. This was checked at 24 sizes from 320 × 568 to 2560 × 1440.

## 8. Reduced motion

With prefers-reduced-motion: reduce:
- the track height becomes 100vh/100svh (no scroll dive)
- the card and nav show immediately with no transition or animation
- Lenis is not created, and the loop does not run
- the scene is rendered once, frozen at p = 0.42 (the arch over the lake), and re-rendered at that point on resize

## 9. Acceptance checklist

- 19 layer images from the 13 proxied URLs above, all loading; Jost 500 loaded.
- The top of the page shows the assembled monastery with the side halls visible between the pushed-out trees, and no seams.
- Scrolling down is one continuous move with no pause at the lake: the layers lift with parallax, and no layer's bottom edge ever shows as a hard line.
- The arch rises from below, frames the lake, then zooms 1.75× into the cave frame. The lake and gate rise into the middle, and the flags fade in hanging in front.
- Every monk stays whole the entire way. The front stair bands ride in front of the upper stairs and never cover the three monks at the hall door.
- The scene is always full screen (no inset card). The nav row is centred at the bottom and rises in on load.
- Scrolling up reverses everything exactly. Lenis is smooth, driven by your own rAF loop.
- The phone, tablet, landscape and desktop layouts all work, and reduced motion shows the still lake view.