---
name: ghibli-inspired-animation-ads
description: Create vertical Ghibli-inspired 2D animated video ads from a product image and advertising angle, using reference-driven character consistency, scene continuity, measured voiceover timing, and controlled video generation.
---

# Ghibli-Inspired Animation Ads

Turn a product image plus an advertising angle into a finished vertical,
Ghibli-inspired 2D animated video ad.

The workflow is a chain of cheap, reviewable artifacts. Each stage anchors the
next, and the user approves at defined gates before expensive generation.
Deterministic operations such as API calls, audio measurement, file handling,
and assembly should live in scripts when available. Your role is planning,
prompt construction, visual quality judgment, continuity management, and
stopping at approval gates.

Use a broad, original hand-painted Japanese animated-film aesthetic. Do not
attempt to reproduce a specific copyrighted film, character, scene, frame,
or exact studio production. The goal is an original visual world with warm
hand-painted 2D animation, watercolor environments, expressive characters,
natural movement, and cinematic storytelling.

---

## Core principles

### 1. Separate the production engine from the visual style

The production workflow is reusable:

Brief
→ Script and plan
→ Style lock
→ Character masters
→ Keyframe stills
→ Voiceover
→ Video clips
→ Assembly
→ Final ad

The Ghibli-inspired direction is a style layer applied throughout that
workflow. Do not redesign the production pipeline for every ad.

### 2. Consistency is a system, not a single prompt

Visual consistency comes from three mechanisms working together:

1. Approved style lock and character masters
2. Sequential scene-to-scene references
3. A two-still clip transition contract

None of these should be treated as optional when the relevant references are
available.

### 3. The real product image is the product authority

The user's actual product image is the only authoritative source for the
product.

Never create a fictional product master.

Whenever the product appears, attach the original packshot/product image as a
reference. Preserve:

- packaging shape
- proportions
- logo
- label layout
- brand colors
- important text
- distinctive product details

Generated product references can progressively corrupt packaging and label
text. If a model produces a distorted product, reject the frame and regenerate
it with a revision note.

### 4. Voiceover determines timing

Generate voiceover per narration line before generating the final clips.

Measure the real duration of every audio file. Use those measurements during
assembly instead of estimating timing from word count.

---

# Requirements

Before starting generation, check the environment.

Required:

- `FAL_KEY` in the environment if the selected workflow uses FAL.
- A product image supplied by the user.
- `ffmpeg` installed for assembly.
- `jq` installed if the available shell scripts require it.

Optional:

- `ELEVENLABS_API_KEY` when the user needs a personal ElevenLabs voice clone
  through the direct ElevenLabs API.

Never ask the user to paste an API key into chat.

If `FAL_KEY` is missing, say so clearly before the first paid generation.
Continue through planning and approval gates that do not require the key.

If authentication fails even though a key appears present, check for invisible
characters such as a UTF-8 BOM (`U+FEFF`) in the environment value or source
file.

Do not invent model names, API payload fields, pricing, or command-line
arguments. Inspect the installed project scripts and current provider/model
documentation when those details are needed.

---

# Project structure

Keep all state inside one folder per ad so a partially completed project can
resume later.

```text
<ad-name>/
  brief.md
  plan.json
  refs/
    style-lock.png
    <character>.png
  stills/
    scene-01.png
    scene-02.png
    ...
  vo/
    line-01.mp3
    line-02.mp3
    ...
    durations.json
  clips/
    clip-01.mp4
    clip-02.mp4
    ...
  jobs/
  final.mp4
```

If the folder already exists, inspect it and resume from the first missing
artifact. Do not restart a completed stage unnecessarily.

---

# Gate 1: Brief

Collect:

- Product name
- What the product does
- Advertising angle
- Target audience
- Product/packshot image path
- Desired call to action, if any
- Approximate ad length, if specified
- Any brand restrictions
- Any required claims or disclaimers

The advertising angle is the single central idea the ad argues.

Write `brief.md`.

Keep the brief concise, ideally under one page.

Show the brief to the user and stop for approval.

Do not spend money before the brief is approved.

---

# Gate 2: Plan and script

Create `plan.json`.

Use this conceptual structure:

```json
{
  "product": {
    "name": "",
    "packshot": ""
  },
  "angle": "",
  "audience": "",
  "visual_world": {
    "rendering": "",
    "palette": "",
    "lighting": "",
    "environment": "",
    "character_grammar": "",
    "animation_behavior": "",
    "negative_constraints": []
  },
  "cast": [],
  "scenes": []
}
```

## Script rules

Use approximately 8–12 narration lines for a standard short ad.

Keep each narration line at or below 10 words whenever possible.

Each narration line becomes:

- one scene
- one voiceover segment
- one keyframe
- one primary clip

Short, declarative lines are preferred because they make timing, visual
storytelling, and editing easier.

Do not force 8–12 lines when the user's requested duration clearly requires
fewer scenes. Adapt the scene count to the actual brief while preserving the
same principle: one clear narration beat per scene.

## Cast rules

Use 1–3 recurring non-product subjects.

Characters can be:

- people
- animals
- personified problems
- organs
- hormones
- emotions
- abstract concepts
- guides
- narrators

Do not cast the product itself as a character unless the user explicitly asks
for a fictional mascot representation separate from the actual product.

For every recurring character define:

- name
- kind
- narrative role
- apparent age, if relevant
- face shape
- eye design
- hairstyle/head shape
- body proportions
- clothing
- colors
- personality
- emotional range
- signature gesture
- immutable identity details

The character description must be detailed enough to create a reliable master
reference.

## Visual world

Every plan must define:

### Rendering

Original hand-painted 2D animated-film aesthetic with:

- painted forms
- delicate brush texture
- subtle line variation
- watercolor/gouache-like environments
- organic shapes
- cinematic composition
- atmospheric depth

### Backgrounds

Use richly illustrated environments appropriate to the story:

- rooms
- streets
- kitchens
- gardens
- forests
- landscapes
- workplaces
- skies
- natural environments

Backgrounds should contain small believable details without becoming visually
cluttered.

### Palette

Choose a harmonious palette based on:

- product branding
- story emotion
- environment
- time of day
- advertising angle

Avoid forcing a fixed palette onto every product.

### Lighting

Prefer:

- natural daylight
- warm sunlight
- soft shadows
- atmospheric haze
- gentle highlights
- believable time-of-day lighting

### Character construction grammar

Keep these stable across all scenes:

- face shape
- eye design
- hairstyle
- body proportions
- clothing
- color treatment
- line treatment
- silhouette
- signature features

### Animation behavior

Prefer restrained, readable motion:

- blinking
- breathing
- head turns
- hand gestures
- walking
- flowing hair
- moving clothing
- moving leaves
- drifting clouds
- steam
- water movement
- gentle camera movement

Avoid chaotic motion that changes character identity or damages product
legibility.

### Negative constraints

Unless the brief specifically requires otherwise:

- no photorealism
- no 3D clay appearance
- no plasticine
- no glossy CGI
- no generic 3D-rendered character look
- no random character redesigns
- no costume changes without narrative reason
- no distorted product packaging
- no invented logos
- no incorrect label text
- no unwanted readable text
- no sudden style changes
- no excessive motion blur
- no harsh neon lighting
- no unnecessary visual effects
- no additional characters unless requested

## Scene planning

Create one scene per narration beat.

Every scene should specify:

- narration
- location
- characters
- character action
- product visibility
- camera framing
- camera movement
- lighting
- environment
- emotional beat
- visual transition into the next scene
- image-generation prompt

The scenes must tell a visual story rather than simply illustrating each
sentence literally.

Present the script and cast list to the user.

Stop for approval.

This is the highest-leverage creative review because changes here propagate
through all later stages.

---

# Gate 3: Style lock and character masters

## Style lock

Generate a style-lock image first.

The style-lock prompt must establish the visual world only.

CRITICAL:

Do not describe recurring characters in the style-lock prompt.

Do not include the product in the style lock.

Do not include logos, labels, or important text.

The style lock should define:

- hand-painted 2D rendering
- brush/paint texture
- watercolor environment treatment
- palette
- lighting
- atmosphere
- environment design
- composition
- cinematic mood

Example direction:

```text
Original hand-painted 2D animated-film environment, delicate watercolor and
gouache textures, expressive painted backgrounds, warm natural sunlight,
soft atmospheric perspective, organic shapes, richly illustrated environment
details, subtle brush texture, cinematic composition, gentle shadows, peaceful
emotional atmosphere, original visual world, no characters, no people, no
animals, no products, no logos, no signs, no readable text, no photorealism,
no 3D CGI, no clay, no plasticine.
```

Treat this as a style reference, not as a final advertisement frame.

## Character masters

Generate each recurring character master after the style lock.

Use the approved style lock as a reference.

The master should establish the character's identity clearly enough that later
scenes can reproduce it.

Each master should preserve:

- face
- eyes
- hair
- body proportions
- clothing
- colors
- silhouette
- distinctive details
- emotional design language

Show the style lock and all character masters.

Ask for approval.

If the user requests a change, record it as:

```text
Revision request: <specific requested change>
```

Regenerate only that artifact.

Once approved, freeze the masters.

Do not casually regenerate an approved master after scene generation begins.
Changing a master can require dependent scene regeneration.

---

# Gate 4: Keyframe stills

Generate scene stills in order.

The sequence is important because every scene uses the previous scene as a
continuity anchor.

## Scene 1

References should include:

- approved style lock
- all relevant character masters
- real product packshot if product is visible

## Scene N

References should include:

- approved style lock
- all relevant character masters
- previous scene still
- real product packshot if product is visible

The prompt must explicitly preserve continuity.

Each image prompt should specify:

- composition
- camera angle
- character action
- environment
- lighting
- emotional state
- product placement
- transition intention
- continuity constraints

Do not allow the model to reinterpret the character from scratch.

## Product accuracy

Whenever the product appears:

- use the actual product image
- preserve packaging geometry
- preserve logo placement
- preserve label colors
- preserve important text
- avoid unnecessary perspective distortion

If packaging becomes visibly wrong, reject the image.

Do not proceed hoping that video generation will repair it.

## Cost gate

Before starting a paid still-generation run:

1. Count the number of images.
2. Obtain the current image-generation price from the active provider/model
   documentation or available API metadata.
3. Calculate the expected cost.
4. Show the user the number and estimated cost.
5. Get explicit approval.

Never fire an unseen paid batch.

## Review

Generate a contact sheet if the project has a contact-sheet script.

The contact sheet should show scene thumbnails together so the user can spot:

- character drift
- inconsistent clothing
- lighting changes
- background discontinuity
- product distortion
- composition problems
- style drift

Show the contact sheet.

Individual scenes may be repaired with a revision request.

If scene N is repaired and the repair visibly changes continuity, inspect later
scenes and regenerate only the downstream scenes that are actually affected.

---

# Gate 5: Voiceover and video clips

Voiceover comes before expensive video generation.

## Voiceover

Generate one audio file per narration line.

Use the project's available TTS script/provider rather than inventing a command
or API schema.

Save:

```text
vo/
  line-01.mp3
  line-02.mp3
  ...
  durations.json
```

Measure the actual duration of every file.

Play one representative line for the user.

Ask the user to confirm:

- voice
- tone
- pacing
- pronunciation

Do not generate the entire expensive video sequence until the voice direction
is accepted.

## Clip count and cost

Before video generation:

1. Count the required transition clips.
2. Include the final outro clip.
3. Obtain the current video-model price.
4. Calculate the estimated cost.
5. Show the user the cost.
6. Get explicit approval.

Video generation is normally the most expensive stage.

Never start a large paid run without showing the user the number first.

---

# Video clip contract

For each transition clip K, use two still images:

```text
[current scene still, next scene still]
```

The prompt must establish this contract:

- first frame matches the first still
- last frame matches the second still
- preserve character identity
- preserve clothing
- preserve environment
- preserve product appearance
- create natural movement between the two states
- do not invent a new visual style
- do not transform the product
- do not introduce unnecessary characters

The clip should feel like a controlled animated transition between two approved
keyframes.

Use the current provider/model's supported duration and parameters. Do not
assume a fixed duration if the active model specifies otherwise.

## Final outro

Generate a final clip for the last scene as a single-reference idle or gentle
animation.

The purpose is to keep the final narration over moving imagery rather than a
visibly frozen frame.

Use restrained movement:

- blinking
- breathing
- subtle camera drift
- environmental motion
- gentle cloth/hair movement

Do not change the final composition substantially.

---

# Assembly

Use the project's assembly script when available.

Conceptually, assembly should:

1. Read measured VO durations.
2. Match each scene to its narration.
3. Trim or extend the associated video appropriately.
4. Freeze-hold the final frame only when necessary.
5. Mux narration into the scene.
6. Concatenate scenes.
7. Normalize final loudness appropriately.
8. Produce a vertical 9:16 final video.

Output:

```text
final.mp4
```

If a music bed is requested and the assembly tooling supports it, duck the
music underneath the narration.

Assembly is a cheap/free iteration compared with regeneration.

After assembly, show the final video.

Offer targeted iterations such as:

- replace one VO take
- repair one scene
- replace one clip
- adjust assembly
- regenerate only affected downstream scenes

Do not regenerate unaffected assets.

---

# Execution behavior

## Resume instead of restarting

At the beginning of every invocation:

1. Check whether the ad folder exists.
2. Inspect existing artifacts.
3. Determine the first missing or invalid artifact.
4. Resume from there.

Do not regenerate approved assets without a reason.

## One generation at a time

When using shell scripts or external generation APIs:

- run one image or clip generation at a time
- keep generation calls in the foreground
- do not create a large local background process
- do not use `nohup`
- do not use `&` to detach generation jobs

If the provider uses server-side jobs, submit the job and poll it using the
available project tooling.

Never invent a job API. Inspect the project's scripts first.

## Artifact visibility

Send/show each important artifact as it becomes available:

1. style lock
2. each character master
3. each scene still
4. voice sample
5. each completed clip when practical
6. contact sheet
7. final video

Do not hide all generated assets until the end.

Approval gates still apply.

## Failure handling

If a generation fails or hangs:

1. Report the failure clearly.
2. Retry once when appropriate.
3. If it fails again, stop and show the error.
4. Do not burn repeated paid retries.

If an image violates a contract:

- reject it
- identify the exact violation
- add a revision request
- regenerate that artifact
- inspect downstream dependencies

Later stages amplify visual mistakes. They should not be expected to repair
them.

If a provider rejects a parameter:

- inspect the active model's current schema/documentation
- inspect the project's API helper/reference files
- correct the parameter
- retry once

Do not assume old API documentation is still current.

---

# Cost discipline

Before every paid stage, state:

- number of images
- image price
- estimated image subtotal
- number of video clips
- video price
- estimated video subtotal
- TTS cost if applicable
- expected total

Verify current provider prices once per session because prices can change.

Do not claim an exact cost when current pricing has not been verified.

Track actual spend in the project folder when the available tooling supports it.

At the end, report the approximate or actual generation spend separately from
free assembly work.

---

# Prompt construction rules

Before writing a generation prompt, inspect any project-specific prompt
templates or reference files that are available.

If the project contains:

```text
references/prompts.md
```

read it before writing generation prompts.

If the project contains:

```text
references/fal-api.md
```

read it before the first FAL API call.

Those files may contain model-specific payloads, routes, prompt templates, and
hard-won consistency rules. Follow them when they are present.

If they are absent, do not pretend their contents are known. Use the workflow
and style rules in this file and inspect the actual installed tooling before
making provider calls.

---

# Ghibli-inspired visual quality checklist

Before approving a still or clip, inspect:

## Style

- Does it look like original hand-painted 2D animation?
- Is the painted/watercolor treatment consistent?
- Is the lighting coherent?
- Does the environment have illustrated depth?
- Has the image accidentally become photorealistic or generic 3D?

## Character

- Same face?
- Same eyes?
- Same hair?
- Same clothing?
- Same body proportions?
- Same colors?
- Same signature details?

## Environment

- Does the location remain coherent?
- Does time of day make sense?
- Does lighting match adjacent scenes?
- Are background details consistent where continuity matters?

## Product

- Correct package shape?
- Correct logo?
- Correct label?
- Correct colors?
- Correct proportions?
- No invented text?
- No melted or distorted packaging?

## Motion

For clips:

- Does the first frame respect the first keyframe?
- Does the final frame reach the next keyframe?
- Is character movement natural?
- Is the camera movement restrained?
- Does the product remain stable?
- Is there unwanted deformation?

Reject anything that fails an important contract.

---

# Recommended creative direction

Use the following qualities as a default starting point, not an inflexible
formula:

- warm
- hand-painted
- whimsical but grounded
- emotionally expressive
- richly illustrated
- natural
- cinematic
- atmospheric
- gentle
- story-first
- visually detailed
- original

The advertising story remains more important than adding decorative animation.

Every visual should help communicate the product's angle.

---

# Important copyright/style boundary

Use wording such as:

> Ghibli-inspired hand-painted 2D animated-film aesthetic

or:

> warm Japanese animated-film aesthetic with watercolor backgrounds and
> expressive hand-painted characters

Do not instruct the model to reproduce:

- a specific Studio Ghibli character
- a specific copyrighted film scene
- an exact frame
- an exact character design
- an exact studio production style

The objective is an original animated world that captures broad visual
qualities rather than reproducing a specific copyrighted work.

---

# Completion criteria

The ad is complete only when:

- `brief.md` exists
- `plan.json` exists
- style lock is approved
- recurring character masters are approved
- all required scene stills are approved
- voiceover files exist and durations are measured
- required video clips exist
- final outro exists
- assembly succeeds
- `final.mp4` exists
- final video has been reviewed
- major continuity/product errors have been resolved

The final deliverable should be a vertical 9:16 video suitable for social
advertising unless the user specifies another format.

---

# Mental model

Think of the Skill as five systems working together:

1. **Creative director**
   Decides the story, angle, characters, world, and visual beats.

2. **Art director**
   Locks the visual language and character design.

3. **Continuity supervisor**
   Prevents characters, environments, and products from drifting.

4. **Production manager**
   Controls generation order, approvals, artifacts, and costs.

5. **Editor**
   Uses measured narration timing to assemble the final advertisement.

Do not skip the creative and continuity decisions simply because the generation
tools can produce an image immediately.

The goal is not to generate many images.

The goal is to generate a small number of controlled, approved assets that
assemble into one coherent animated advertisement.
