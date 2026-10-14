# Project file

A Motionboard project is one JSON file. The Motionboard Mac app opens the same files (Open Animation…).

```json
{
  "id": "proj_x1y2z3",
  "name": "Launch teaser",
  "settings": {
    "width": 1080, "height": 1920, "fps": 30,
    "backgroundPreset": "cream", "bgBase": "#F7F3EE", "bgGlow": "#F1B59B", "glowStrength": 0.85,
    "accent": "#E8845A", "textColor": "", "textOnBg": "",
    "corners": "rounded", "shadowTone": "warm",
    "fontFamily": "Inter", "fontSource": "bundled",
    "tone": "professional", "smoothness": "smooth", "constantMotion": false,
    "entrance": "pop", "pace": "normal"
  },
  "scenes": [ /* see scene-contract.md */ ],
  "assets": [ { "id": "asset_abc", "name": "acme-logo", "url": "/api/assets/asset_abc.png", "width": 512, "height": 512, "kind": "logo" } ],
  "source": { "file": "clip_…mp4", "url": "/api/assets/media/clip_…mp4", "name": "take-3.mp4", "duration": 42.1, "width": 1080, "height": 1920, "transcript": { "text": "…", "segments": [ { "start": 0, "end": 3.1, "text": "…" } ] } },
  "mode": "broll",
  "brandId": "brand_…"
}
```

## Settings
- **Format:** `width`/`height` (output size). 1920×1080, 1080×1920, 1080×1080 or 3840×2160; a clip sets its own shape. The design canvas derives from this (formats.md).
- **Look:** `bgBase`/`bgGlow`/`glowStrength` (background), `accent`, `textColor` (text on cards; `""` = automatic), `textOnBg` (text on the background; `""` = automatic), `corners` `rounded|soft|sharp`, `shadowTone` `warm|neutral|accent` (use `neutral` over footage), `fontFamily` + `fontSource` (`bundled` Inter, `google` with a Google Fonts family, `system` for SF Pro, `local` for an installed font by name).
- **Motion:** `tone` `playful|professional|sleek`, `smoothness` `crisp|smooth|silky`, `constantMotion`, `entrance` `pop|rise|fade|slide|drop`, `pace` `calm|normal|snappy`.

A scene can override any of these for itself with `"style": { … }`.

## Timing
- No clip, or `mode: "full"`: scenes play back to back in array order; `duration` is seconds. With a clip in full mode, make durations add up to the clip's length, cutting on sentence boundaries from the transcript.
- `mode: "broll"`: each scene has `"start"` (seconds on the clip) and `"duration"`; no overlaps, leave plain footage between them.

## Assets
Images live in the engine's data folder (shared with the Motionboard app when it's installed). Brand kit assets come with `mb new --brand`. To use another image, add it in the editor (Assets tab) or ask the creator to.
