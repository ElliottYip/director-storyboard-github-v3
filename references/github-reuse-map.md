# GitHub Reuse Map

Use this file when deciding which public workflow patterns to reuse. Reuse structures, schemas, and QA gates; do not blindly copy visual style or platform claims.

## Core Short-Drama Workflows

| Source | Reuse | Do Not Reuse Blindly |
|---|---|---|
| `HBAI-Ltd/Toonflow-app` | Chapter event graph, ScriptAgent/ProductionAgent split, canvas nodes for materials/characters/scenes/storyboards/videos, persistent memory, provider adapters, cost tracking | App UI assumptions or default visual style |
| `Forget-C/Jellyfish` | Consistency-first entity model: characters/actors/scenes/props/costumes, shot preparation, candidate confirmation, shot ready, generation workspace, task center | Exact implementation details without reading repo |
| `0xsline/StoryGen-Atelier` | Sliding-window transition analysis: Shot A -> Shot B, transition prompt, duration, start/end frames, concat | Treating every transition as Veo-specific |
| `ArcReel/ArcReel` | Novel -> characters/clues -> episodes -> script JSON -> character designs -> storyboards -> clips -> composition | Any unverified platform facts |
| `Open-AI-Micro-Drama-Generator` | Screenwriter -> character extraction -> storyboard -> portraits -> first frames -> I2V clips -> concat, parallel generation | Prompt depth; it is shallow as director guidance |

## Prompt / Skill Libraries

| Source | Reuse | Caution |
|---|---|---|
| `Emily2040/seedance-2.0` | Director engine, episode state, continuation source gate, failure atlas, retake protocol, reference role map | Not official Seedance facts |
| `crowscc/seedance-director` | 15s segmentation, video extension, `@` reference usage, six-part prompt structure, scene strategies | Low-star but highly relevant; verify hard facts |
| `AGI-Ruby/ai-GPT_Image2-Seedance_2.0-video-skills` | Two-stage GPT Image storyboard -> Seedance video handoff, character reference locks identity, storyboard locks shot order | Platform names and API assumptions need verification |
| `Arch-Dog/video-prompt-engineer` | Audio ledger, 10-15s consistency container, transition anchors, emotion externalization, independent executable prompts | Community skill, not official docs |
| `Alisa0808/vibe-creating-skill` | Judgment-first rewrite, visual anchor / action-state / local tonality / video theme, translating technical parameters into visible results | Do not use for strict dialogue sync or exact shot-by-shot tasks |
| `LichAmnesia/awesome-ad-video-prompts` | Multi-beat timing, real camera language, fidelity guards, implied sound | Advertising/product tone conflicts with historical drama |

## Control / Blender / Geometry

| Source | Reuse | Caution |
|---|---|---|
| `iLearn-Lab/DramaDirector` | Depth/pose assets, storyboard schema, first-frame generation, video synthesis, reward/reviewer loops | New/low-star but highly aligned with complex short drama |
| `DrawVideo` | Sketch + appearance prompt + motion prompt separation | Research workflow, not direct platform adapter |
| `AnyI2V`, `Wan-Move`, `I2VControl-Camera`, `UCPE` | Trajectories, camera pose/control, object/camera motion separation | Do not claim exact execution unless target workflow is chosen |
| `kijai/ComfyUI-WanVideoWrapper` | Workflow hints: FLF2V, Fun Camera, WanMove, WanAnimate, ReCamMaster, pose/depth/control paths | README says WIP/sandbox; read workflow JSON before execution |

## Image / Identity / Style

| Source | Reuse | Caution |
|---|---|---|
| `StoryDiffusion` | Generate consistent character/state images before videos; use condition images for long video | Not a one-click guarantee |
| `InstantID`, `PuLID`, `Character-Adapter` | Identity reference, landmarks/keypoints, region/role separation, ID fidelity vs editability tradeoff | Multi-person scenes need separate references/regions |
| `StyleAligned`, `AlignedGen` | Shared style reference/attention concepts; style consistency cannot rely on same words only | Execution depends on model/workflow support |
| GPT Image prompt libraries | JSON fields: identity_lock, environment, lighting, camera, negative prompts | Do not copy viral styles, HDR/8K/beauty language |

## Reuse Rule

When adapting a GitHub source:

1. Extract only one of: schema, state machine, checklist, prompt field, control route, retake rule.
2. Mark evidence type: official / GitHub workflow / research / third-party platform / community sample.
3. Keep hard platform facts out unless officially verified.
4. Translate commercial/viral style words into the current Visual Bible.
5. If a source solves an exact failure mode, include that failure mode in `retake.failed_dimension`.
