---
name: motionboard
description: Create original, editable motion graphics with Motionboard. Turn a script, a voiceover, or a video clip into animated scenes (stats that count up, comparisons, UI mockups, step flows, lower thirds, kinetic type) as full animation or as B-roll timed over the footage, in 16:9, vertical 9:16 or square, in the creator's brand kit. Renders MP4 or transparent ProRes 4444 for Final Cut and Premiere, and opens a visual editor where every scene stays editable. Use when someone says "/motionboard", asks for motion graphics, animated B-roll, overlays or lower thirds for a video, or wants graphics generated from a script or clip, including midway through another video-editing workflow.
---

# Motionboard

You design the scenes yourself. Motionboard supplies the engine: a project format, a renderer, a self-test, and an editor the creator can use alongside you. Every scene you write is HTML + CSS + a GSAP timeline that the engine seeks frame by frame, so renders are exact and every scene stays editable.

**Engine:** `<this skill's base directory>/engine/mb` (a bash script; call it with its full path, quoted, since it may contain spaces). The first call installs its dependencies once (Node 18+ required; a headless browser is downloaded the first time, a few minutes). Run `mb` with no arguments for the command list.

```
mb new <project.json> [--format 16:9|9:16|1:1] [--brand "Kit"] [--name "Title"]
mb clip <project.json> <video> [--mode broll|full] [--no-transcribe]
mb check <project.json>              # self-test every scene; exit code 2 means fix something
mb snapshot <project.json> [--scene N] [--at s]   # PNGs of each scene's finished frame; look at them
mb render <project.json> [--format mp4|prores4444|prores422|webm] [--transparent] [--no-clip] [--scene N] [--out dir]
mb edit <project.json>               # visual editor on http://127.0.0.1:5181, live-synced with the file
mb brands                            # the creator's brand kits (from the Motionboard Mac app)
```

## Workflow

1. **Brief.** From the request, settle: the narration (script, transcript, or a clip), the **format**, the mode with a clip (B-roll over the footage, or full animation over its audio), the look (a brand kit from `mb brands`, or colors/font/personality), and the output (MP4 to post, or transparent ProRes 4444 to stack over footage in Final Cut). Ask only for what you can't infer. Format matters most: never design widescreen and squeeze it into vertical.
2. **Project.** `mb new work/<name>.json --format 9:16 [--brand "Kit"]`. With a clip: `mb clip work/<name>.json clip.mp4 --mode broll`. It copies the clip in, sets the output to the clip's shape, and transcribes it with timestamps when an OpenAI key is available (env `OPENAI_API_KEY`, or the key saved in the Motionboard app). Without a transcript, ask for the script.
3. **Plan.** Split the narration into beats and write a concept per scene: layout, the main visual device, what animates, the exact short on-screen words. Vary devices and compositions scene to scene. For B-roll: pick the moments graphics help (numbers, lists, named tools, comparisons, steps), 3–8 s each, starting ~0.2 s before the words, with gaps of plain footage; choose `cover: "full"` (cut away) or `"overlay"` (lower third, side card over the speaker).
4. **Design.** Write each scene into the project's `scenes` array following [reference/scene-contract.md](reference/scene-contract.md) and the canvas guidance in [reference/formats.md](reference/formats.md). The project file format is in [reference/project-format.md](reference/project-format.md). With a brand kit, read its `styleGuide` and look at its `references` images; borrow their style, never their colors, fonts or words.
5. **Test and look.** `mb check` and fix every problem it lists, then `mb snapshot` and look at each PNG with your own eyes: hierarchy, crowding, things cut off, whether it reads in two seconds. Iterate until both are clean. This is the part that makes the work good; don't skip it.
6. **Hand over.** `mb render …` for the deliverable, and offer `mb edit` (run it in the background) so the creator can tweak text, swap logos and screenshots, drag elements, and change style per scene. The editor saves to the same file and reloads when you change it, so you can keep editing on request while it's open. In the Claude desktop app you can show the editor in the browser pane.

## Working inside other workflows

Motionboard is a step, not a silo. Mid-edit you can make a single lower third, an animated stat, or a B-roll pass for one clip, then hand back the rendered file. Video from other tools (a generated clip from Higgsfield or similar, a screen recording, a talking-head take) goes in with `mb clip` and gets graphics timed to it. For a graphic to layer in an editor, render `--format prores4444 --transparent --no-clip`: it runs the clip's full length with transparency between graphics, so it lines up at 0:00 above the footage.

## Rules that matter

- Coordinates are in the **canvas** for the format (1920×1080, 1080×1920, 1080×1080, or the clip's shape with a 1080 short side), never the output resolution.
- Colors, radii, shadows and the font come only from the `--mb-*` CSS variables, so style changes (and brand kits) restyle every scene without rewriting it.
- All motion lives on `ctx.tl`. Anything else (CSS animations, timers, `Math.random`, network) is rejected by the engine because renders seek frame by frame.
- Put every on-screen word in a `{{field}}` so the creator can edit it in the editor; put logo and screenshot spots in `data-slot` elements.
- Templates exist in the format (`template` other than `"generated"`), but original scenes are the point; write originals.
