# Ghibli-Inspired Animation Ads — a Claude Skill

By Avirup Sarker

A Claude Code skill to create Ghibli-style animation video ads for any brand. Create video ads for peanuts compared to Higgsfield and Sedance without losing out on quality.

Turn a product photo and an advertising angle into a finished vertical Ghibli-inspired animated video ad, entirely inside Claude.

Claude runs the full production pipeline: writing the script, designing characters in a hand-painted 2D style, generating every keyframe still, recording the voiceover, animating clips between keyframes, and stitching the final video. You review at five checkpoints and approve the spend before anything expensive runs.

A typical 10-scene ad costs around $5-7 in API credits and takes about 30 minutes start to finish.

## Download

[Download ghibli-animation-ads.zip](ghibli-animation-ads.zip)

## Install

1. Open the Claude desktop app
2. Go to **Customize > Skills > Add > Upload a skill**
3. Upload `ghibli-animation-ads.zip`

## Requirements

A [fal.ai](https://fal.ai) API key. Get one at [fal.ai/dashboard/keys](https://fal.ai/dashboard/keys). It's pay-as-you-go, no subscription needed. You only pay for what you generate.

**ffmpeg** and **jq** for assembling the final video. On Mac:

```bash
brew install ffmpeg jq
```

On Windows, install ffmpeg from [ffmpeg.org](https://ffmpeg.org/download.html) and jq from [jqlang.org](https://jqlang.github.io/jq/download/).

**Optional:** an `ELEVENLABS_API_KEY` if you want to use your own cloned voice. Stock voices work through fal.ai without it.

## Use it

Give Claude your product image and the angle you want the ad to argue:

> I sell a testosterone product for men called Mars Men — image attached. Make me a Ghibli-style animation ad based on the below:
>
> 45 second ad. Testosterone levels have been dropping 1% every year since 1980. Stress, poor sleep, processed diets, and toxins are destroying natural hormone production. Low testosterone drains energy, strength, and focus. Mars Men helps you reclaim your edge.

Claude takes it from there.

## How it works

The look is a warm, hand-painted 2D animated-film aesthetic. Think watercolor backgrounds, expressive brush textures, soft natural lighting, and cinematic compositions. It draws from broad Japanese animation traditions without copying any specific studio or film.

Character consistency across a 10-scene ad is the hard problem. Three mechanisms stack to solve it:

**Approved masters ride every image.** A style lock (the visual world) and one master per recurring character are generated first, then attached as references to every later generation. This keeps the palette, brush style, and character design locked down from scene one.

**The chain.** Each scene also references the previous scene's still, so adjacent frames agree on lighting, environment, and character appearance. Drift gets caught early because continuity errors compound.

**The clip contract.** Each video clip is generated from a pair of keyframe stills, with a prompt binding the first frame to one still and the final frame to the next. When clips join end-to-end, the transitions are seamless because the exit frame of one clip matches the entry frame of the next.

And the rule that outranks all three: your real product photo is the only product authority. It's attached to every frame where the product is visible, so the packaging, logo, and label never degrade across scenes.

Voiceover is generated before the clips, so scene timing comes from measured audio rather than guesswork. That's why the final cuts land exactly on the narration without any manual timing adjustment.

## The five approval gates

| Gate | What you approve | Cost |
|------|-----------------|------|
| 1 | The brief | free |
| 2 | Script, cast, and visual world | free |
| 3 | Style lock and character masters | ~$0.30 |
| 4 | All scene keyframes | ~$0.60 |
| 5 | Voice sample, then the video clips | ~$4.00 |

Assembly is free and re-runnable, so fixing one clip and re-stitching costs nothing extra.

## Models

- **Images** — fal.ai nano-banana (style lock, character masters, scene keyframes)
- **Video** — Vidu Q1 start-end-to-video (keyframe-to-keyframe transitions)
- **Voice** — ElevenLabs via fal.ai

## What makes this different from claymation ads

This skill uses the same production pipeline structure as the [claymation ads skill](https://github.com/mikefutia/claymation-ads-claude-skill), but swaps the visual style entirely. Instead of 3D clay-textured characters, everything here is flat 2D with painted textures, watercolor environments, and soft hand-drawn line work. The look feels more like a warm animated short film than a stop-motion commercial.

The underlying consistency system (style locks, character masters, scene chaining, clip contracts) works the same way. If you've used the claymation version, you already know the workflow. The difference is purely visual.

## Notes

Every project lives in its own folder with all state on disk, so you can stop mid-pipeline and resume in a later session. fal.ai's queue runs server-side, so a submitted job survives even if you close your laptop.

If a generation comes back looking wrong, tell Claude what's off and it regenerates just that one artifact with a revision note. Fixing one scene doesn't invalidate the rest.

The skill creates helper scripts (`gen_image.py`, `gen_video.py`, `tts.py`, `gen_clips.py`, `assemble.py`) inside a `tools/` folder in your project directory. These handle the actual API calls and ffmpeg assembly so everything is reproducible and resumable.

Built by Avirup Sarker. Free to use and modify.
