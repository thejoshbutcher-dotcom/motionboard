# Motionboard for Claude Code and Codex

Ask for motion graphics and get original, editable animated scenes: stats that count up, comparisons, UI mockups, step flows, lower thirds, kinetic type. They're designed for what you're saying, timed to your video if you have one, in your brand, and rendered to MP4 or transparent ProRes 4444 that drops straight into Final Cut or Premiere.

The AI you're already talking to designs every scene. Motionboard supplies the engine: renderer, self-test, and a visual editor that stays in sync, so you can tweak anything by hand while the AI keeps working.

## Install

**Claude Code:** paste this repo's link and say "install this", or run:

```
/plugin marketplace add thejoshbutcher-dotcom/motionboard
/plugin install motionboard@motionboard
```

Then type `/motionboard`, or just ask for motion graphics at any point, including in the middle of another workflow.

**Codex:** add the repo as a plugin (it ships a `.codex-plugin` manifest and `.agents/skills/motionboard`), or copy `skills/motionboard` into your Codex skills folder.

**Any agent that reads skills:** copy `skills/motionboard` into its skills directory. The skill is self-contained; the engine lives in `skills/motionboard/engine`.

Requirements: macOS or Linux, Node.js 18+. The first run installs the engine's dependencies and a headless browser once (a few minutes). Transparent ProRes uses Apple's encoder on macOS.

## What you can ask for

- "Make vertical motion graphics for this script" (paste the script)
- "Add B-roll graphics over interview.mp4: a little, mostly lower thirds"
- "Turn this voiceover into a full animation, 16:9, in my JBO brand kit"
- "Just one lower third: 'Josh Butcher, filmmaker'. Transparent ProRes."
- "Make scene 3 bigger and slower" (keeps editing the same project)

## How it works

- **Formats:** 16:9, 9:16 and 1:1 are designed natively, not cropped. A clip sets the format to its own shape.
- **Clips:** the clip is copied into the project and transcribed with timestamps (needs an OpenAI key: `OPENAI_API_KEY`, or the key saved in the Motionboard app), and graphics are timed to what's said. Use B-roll mode over the footage, or full animation over its audio.
- **Brand kits:** if the Motionboard Mac app is installed, the skill reads its brand kits (colors, fonts, logos, style references and style guide) and shares its uploaded assets.
- **Self-test:** every scene is checked for script errors, things cut off at the edge, unreadable or overlapping text, overlays covering the speaker, and filler words, and the AI looks at a snapshot of each scene before rendering.
- **Editor:** `mb edit` opens the Motionboard editor on the project at http://127.0.0.1:5181. Edits save to the project file, and changes the AI makes appear live.
- **Project files** are the Motionboard app's format, so you can also open them in the app.

## Engine commands

`skills/motionboard/engine/mb` (the skill runs these for you):

```
mb new <project.json> [--format 16:9|9:16|1:1] [--brand "Kit"]
mb clip <project.json> <video> [--mode broll|full]
mb check <project.json>
mb snapshot <project.json> [--scene N]
mb render <project.json> [--format mp4|prores4444|prores422|webm] [--transparent] [--no-clip]
mb edit <project.json>
mb brands
```

The engine is built from the Motionboard app's source (`npm run skill:build` in the app repo).
