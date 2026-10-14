# Scene contract

An original scene is an entry in the project's `scenes` array with `"template": "generated"`. Its `props` hold the design:

```json
{
  "id": "s_hours",
  "template": "generated",
  "duration": 4.5,
  "beat": "Most editors spend six hours on graphics for every video.",
  "props": {
    "status": "ready",
    "canvas": "1080x1920",
    "concept": "Big 6 HOURS counter over a stack of video cards",
    "summary": "vertical hero counter above stacked video cards",
    "html": "<div class=\"kicker\">{{kicker}}</div><div class=\"hero\"><span class=\"num\">{{hours}}</span><span class=\"unit\">{{unit}}</span></div>",
    "css": ".kicker{position:absolute;left:96px;top:300px;font:700 40px var(--mb-font);color:var(--mb-accent-deep)} .hero{position:absolute;left:96px;top:370px;display:flex;align-items:baseline;gap:28px;color:var(--mb-on-bg)} .num{font:800 300px/0.9 var(--mb-font);letter-spacing:-.05em} .unit{font:800 120px var(--mb-font)}",
    "code": "const {tl,q,ease,P}=ctx;\ntl.from(q('.kicker'),{y:30,autoAlpha:0,duration:P(0.5),ease:ease.out},0);\nconst num=q('.num'); if(num) ctx.count(num,num.textContent,P(0.15),P(1.1));\ntl.from(q('.hero'),{y:60,autoAlpha:0,duration:P(0.7),ease:ease.pop},P(0.1));",
    "fields": [
      { "key": "kicker", "label": "Kicker", "default": "Every single video" },
      { "key": "hours", "label": "Number", "default": "6" },
      { "key": "unit", "label": "Unit", "default": "hours" }
    ],
    "values": {},
    "slots": [],
    "slotValues": {},
    "seed": 1
  }
}
```

- `canvas` records the canvas you designed for (see formats.md). If the format changes, the editor offers to redesign scenes whose canvas differs.
- `fields`: every `{{key}}` in the html must have a field. `values` holds the creator's edits; leave it `{}`.
- `slots`: `[{ "key": "logo", "label": "Logo", "kind": "logo" | "image" }]` for every `data-slot`. `slotValues` maps slot keys to asset ids from the project's `assets` (for a brand kit, its logo id fills logo slots).
- B-roll scenes also carry `"start"` (seconds on the clip) and `"cover": "full" | "overlay"`.
- `status` is `"ready"` once designed.

## Canvas

- The canvas matches the video's shape (formats.md gives sizes and composition notes). Design natively for it.
- `box-sizing: border-box` everywhere; headings, paragraphs and lists have no default margins; base font-size 16px.
- The html is inserted directly into a root of the canvas size (`position: relative; overflow: hidden`). Every **top-level** element must be `position: absolute` with explicit `left`/`top` in px in the CSS; the editor lets the creator drag top-level elements. Nest freely inside them.
- `cover: "full"`: the engine draws the background (base color with soft glows) across the canvas. Draw only the foreground, unless the concept needs a colored panel.
- `cover: "overlay"`: the scene sits over live video of a person talking. Keep at least 55% of the frame clear and keep the face zone (formats.md) clear. Lower thirds, side cards, corner callouts, on solid `var(--mb-card)` with shadows so they read over video. Never full-frame.

## Design system

Use these CSS variables for every color, radius, shadow and the font:

`--mb-bg` (background), `--mb-on-bg` (text directly on the background), `--mb-on-bg-soft`, `--mb-card` (card surface), `--mb-ink` (text on cards), `--mb-ink-soft`, `--mb-accent`, `--mb-accent-deep`, `--mb-accent-soft`, `--mb-bar` (skeleton line), `--mb-bar-mid`, `--mb-chip`, `--mb-track`, `--mb-dark` (dark surface), `--mb-dark-2`, `--mb-dark-text`, `--mb-r-card`, `--mb-r-window`, `--mb-r-small`, `--mb-r-pill`, `--mb-r-button`, `--mb-shadow`, `--mb-shadow-soft`, `--mb-font`.

- `font-family: var(--mb-font)` everywhere. Weights 500–800. Confident type: hero 96–160 px (110–180 in vertical), headings 56–84, labels 26–40, negative letter-spacing on large type. Nothing under 22 px.
- Look: clean UI illustration: cards, chips, pills, skeleton text lines standing in for body copy, charts, counters, arrows and connectors, cursors, device and browser frames, toggles, progress bars, simple geometric shapes, inline SVG icons (stroke-based, round caps, `currentColor` or `var(--mb-accent)`). No emoji, no photos, no external URLs, fonts or images.
- Generous spacing, clear hierarchy, align to a grid, fill the frame purposefully (formats.md gives the target span).

## Text and slots

- On-screen words come from the beat: short (≤ 6 words per element). Never paste narration sentences, never filler ("Card 1", "Item", "Label", "Lorem").
- Legibility: on cards `--mb-ink`/`--mb-ink-soft`; on the background `--mb-on-bg`/`--mb-on-bg-soft`; on dark or accent fills `--mb-dark-text` or white. `--mb-accent` text on a light background is often too faint; use `--mb-accent-deep`.
- Keep every element at least 40 px inside the frame at its final position; no text overlapping text, rings or bars; text never spills out of its container.
- Logo or screenshot spots: an element with `data-slot="key"` (add `data-fit="cover"` for screenshots) containing a tasteful fallback (a rounded square with a simple SVG mark; skeleton page blocks). The engine swaps in the image the creator picks.

## Code

`code` is the **body** of `function (ctx) { … }`. On `ctx`:

| | |
|---|---|
| `root`, `tl` | the scene root; the GSAP timeline (position 0 = scene start) |
| `q(sel)`, `qa(sel)` | scoped querySelector / querySelectorAll (`qa` returns an Array) |
| `duration`, `cover`, `tone`, `W`, `H` | scene length (s), `"full"`/`"overlay"`, `"playful"`/`"professional"`/`"sleek"`, canvas size |
| `ease` | `{ out, in, inOut, pop, soft, bounce }` tuned to the project's smoothness and tone; use these, not your own ease names |
| `P(seconds)` | scales a duration or offset by the project pace; wrap timings in it |
| `rand()` | seeded 0..1 (never `Math.random`) |
| `split(el, "words" \| "chars")` | inline-block spans for staggering |
| `type(el\|sel, text, at, dur?)` | typewriter |
| `count(el\|sel, finalText, at, dur?)` | count-up keeping prefix/suffix (`"$10,500"`, `"98%"`, `"2.4x"`) |
| `draw(svgEl\|sel, at, dur?)` | stroke draw-on |
| `gsap` | only `gsap.set` and `gsap.utils` |
| `motion.constant` | the creator wants continuous motion |

Rules:

- Every tween goes on `ctx.tl` with an explicit numeric position: `tl.from` / `tl.to` / `tl.fromTo` / `tl.set`. Elements sit in their final state in the CSS; bring them in with `tl.from`, so the final frame equals the layout.
- Rejected by the engine: `gsap.to/from/timeline/delayedCall`, `setTimeout`, `setInterval`, `requestAnimationFrame`, CSS animations or transitions, `Date`, `Math.random`, `fetch`, `window`, `document` (except `createElement`/`createElementNS`), assigning `innerHTML`, `repeat: -1`.
- Finite loops only: `repeat = Math.max(0, Math.floor((duration - start) / cycle) - 1)`.
- Timing: build the idea in the first 50–65% of the scene, then hold the complete, readable state. Nothing exits before `duration - 0.6` (the engine fades scenes out). Entrances 0.5–0.9 s, staggers 0.05–0.12 s. Choreograph: frame, then the hero, then supporting details, then one accent moment (a highlight, a check, a counter landing, a line drawing on).
- With `motion.constant`, keep gentle finite motion after things land (4–12 px float, a slow ring rotation, a shimmer along a bar); otherwise hold still.
- Never throw: guard lookups (`if (!el) return;`). Keep code under ~120 lines.

## Originality and tone

- Invent a visual metaphor specific to the beat. Vary layout, composition and the primary device from scene to scene: typography-led statements, count-up stats, bar/line/donut charts, before/after splits, step flows with connectors, stacked cards, UI mockups with a cursor, device or browser frames, timelines, comparison tables, radial diagrams, checklists, receipts, toggles and sliders.
- `playful`: bouncy overshoot, rounded blobby shapes, bright accent fills, slight rotations, sticker-like chips. `professional`: restrained, grid-aligned, data and structure, little overshoot. `sleek`: minimal, lots of negative space, thin lines, oversized type, long smooth eases, subtle glows.
- A brand kit's style guide and reference images describe the visual language to borrow (composition, surfaces, iconography, motion); colors and the font still come from the variables. Stay 2D: if a reference implies 3D or photos, use a flat or layered 2D treatment with the same feel.
