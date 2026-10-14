# Formats and canvases

The design canvas follows the output shape: short side 1080 px, long side by aspect. Read it from the project (`settings.width/height`) or from `mb new`/`mb clip` output.

| Format | Canvas | Typical use |
|---|---|---|
| 16:9 | 1920×1080 | YouTube, Final Cut timelines |
| 9:16 | 1080×1920 | Reels, TikTok, Shorts |
| 1:1 | 1080×1080 | Feeds |
| other (e.g. a 4:5 clip) | 1080×1350, etc. | matches the clip |

## Widescreen (16:9)
- Side-by-side comparisons, wide flows and asymmetric layouts all work.
- The main group spans about 45–75% of the width (860–1440 px).
- Overlay face zone (keep clear): x 595–1325, y 76–800.

## Vertical (9:16)
- Compose top-to-bottom. One idea per screen; stack cards and rows instead of placing them side by side.
- For `cover: "full"`, fill the safe area: the composition feels big and centered in it, never a small card floating in a tall frame.
- Hero text 110–180 px; the main group spans about 75–90% of the width (810–970 px). Side margins at least 64 px.
- Keep the top 192 px and the bottom 365 px free of important content: phone apps put their UI and captions there.
- Overlay face zone (keep clear): x 194–886, y 422–1114. Put overlays in the upper band (y 211–422) or the lower band (y 1152–1536).

## Square (1:1)
- Centered compositions, a single hero, or a 2×2 grid. Avoid wide horizontal flows.
- The main group spans about 60–85% of the width (648–918 px). Margins at least 64 px.
- Overlay face zone (keep clear): x 270–810, y 130–756. Overlays go in the bottom third or a top corner.

## Clips
`mb clip` sets the output to the clip's shape. In B-roll mode the clip plays underneath: `cover: "full"` scenes cut away (the engine draws a background behind them), `cover: "overlay"` scenes sit on the footage. In full mode the graphics replace the picture for the whole clip and its audio plays underneath.
