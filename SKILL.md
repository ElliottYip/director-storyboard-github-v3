---
name: director-storyboard-github-v3
description: Maximum-quality AI video director skill for Seedance, Jimeng, GPT Image 2, KIE, Higgsfield, and similar image/video workflows. Use when converting scripts or story ideas into structured video prompts, shot plans, editable HTML shotlists, reference-image roles, storyboard sheets, first-frame/end-frame prompts, transition plans, retake diagnosis, or historical-drama/short-drama production assets. V3 adds markdown RAG routing for video prompt templates: action micro-detail, micro-expression, camera motion, lighting/material, sound, negatives, reference-image usage, failure-specific retake rules, and optional 15-second copy-ready shotlist delivery.
---

# Director Storyboard GitHub V3

Create production-ready AI-video packages with deterministic structure. V3 keeps V2's visual bible / reference role / shot-ready gates, adds a Markdown RAG layer for video prompt generation so complex prompts are assembled from shot-type templates instead of free-form improvisation, and can package final prompts into an editable director shotlist when the user needs a shooting/generation checklist.

Default to **maximum quality**, not quick drafting.

## Core Workflow

1. **Style Calibration**: ask proactively when missing style/platform/output constraints would materially change the result. Record a project-level `style_contract`; do not bake one project's style into the skill.
2. **Visual Bible**: fixed palette, histogram, material, skin, camera texture, forbidden looks.
3. **Entity Registry**: characters, scenes, props, costumes, state changes.
4. **Asset Manifest**: hero/four-view, action states, scene empty plates, first frames, optional end frames, storyboard sheets.
5. **Asset Confirmation Gate**: before generating image assets or treating missing references as final, confirm what assets to create, reuse, or skip.
6. **Reference Role Manifest**: every image gets exactly one role: `identity`, `costume_structure`, `environment`, `first_frame`, `storyboard_order`, `blocking_route`, `pose_depth_canny`, `style`, or `audio`.
7. **Shot Ready Gate**: do not write final video prompts until minimum references are available.
8. **Video RAG Routing**: classify shot type, load the matching `references/video-rag/*.md`, then assemble the prompt.
9. **Structured Video Prompt**: output in Chinese by default when the user is working in Chinese.
10. **Shotlist Assembly**: when the user asks for a shotlist, shooting plan, copyable prompts, or HTML deliverable, split the story into numbered scenes and 4-15s prompt chunks, then package the structured prompts into a self-contained editable HTML file.
11. **User Review Gate**: before final prompt batches, image generation, or HTML delivery, briefly show the intended style/assets/shot split when those choices are consequential.
12. **Transition Builder**: every adjacent clip needs sound/object/gaze/light/occlusion/action bridge or an explicit independent cut reason.
13. **Retake Discipline**: diagnose first; change one variable per retake.

## Director Style Menu

When style is missing or the user asks what styles are possible, offer a compact menu instead of inventing silently. Do not force the menu when the user's style is already clear.

Common style presets:

- **现实电影感**: restrained acting, natural light, grounded spaces, lens-motivated camera.
- **短剧强情绪**: faster emotional turns, clearer reaction beats, stronger close-ups, vertical-friendly blocking.
- **古装史诗**: ritualized blocking, layered depth, robes/armor/material discipline, large environmental scale.
- **武侠/玄幻**: body rhythm, cloth/hair motion, controlled VFX, gravity/inertia rules.
- **悬疑黑色电影**: low-key contrast, withheld information, silhouettes, slow reveals, motivated shadows.
- **纪录片观察感**: handheld restraint, natural imperfection, imperfect framing, unsentimental timing.
- **广告产品片**: clean material highlights, product-first blocking, simple camera moves, no narrative clutter.
- **动画/风格化**: clear shape language, palette discipline, stylized motion rules, no accidental realism.

Style selection output:

```text
style_contract:
  style_preset: [...]
  medium: [...]
  palette: [...]
  camera_texture: [...]
  acting_register: [...]
  forbidden_looks: [...]
```

If none fits, ask the user for one sentence of taste direction, or propose a hybrid like `现实电影感 + 短剧强情绪`.

## Conditional Clarification

Default to inferring and moving forward, but ask the user when a missing decision would create a meaningfully different production package.

Ask concise questions only for blocking or high-impact unknowns:

- **Style / genre**: realism, period drama, wuxia, noir, documentary, commercial, animation, stage recording, etc.
- **Platform / model**: Seedance, Higgsfield, Jimeng, KIE, GPT Image 2, or unknown.
- **Output form**: direct prompts, shot table, HTML shotlist, reference-image plan, first-frame prompts, retake diagnosis.
- **Aspect ratio / duration**: vertical short drama, horizontal cinematic, square social, 4-15s clips, 15s fixed chunks.
- **Reference assets**: whether identity, costume, environment, first frame, storyboard order, or audio references exist.
- **Language**: Chinese planning with Chinese prompts, Chinese planning with English copy prompts, or fully English package.

Do not run a questionnaire by default. Ask at most three questions, choose sensible defaults for the rest, and state those defaults briefly before continuing. If the user says "你定", "直接做", "先来一版", or gives a terse correction, infer the missing pieces and proceed.

If references are missing but the user still wants a draft, produce a clearly labeled `draft shotlist / reference plan` instead of pretending it is shot-ready. Final video prompts still require the Shot Ready Gate unless the user explicitly accepts a lower-control draft.

## Confirmation Gates

Do not ask for confirmation at every step. Use confirmation only at gates where the answer changes cost, asset count, shot count, or final creative direction.

Required gates:

1. **Style Gate**: confirm or propose style when style/genre is absent or contradictory.
2. **Asset Gate**: before generating image assets, ask whether to create/reuse/skip identity, costume, environment, first-frame, end-frame, storyboard, or audio references; also ask whether to call a generator with references attached or only export image prompts for the user to generate manually.
3. **Shot Split Gate**: for long scripts, show the planned scene/chunk split before writing a large batch of final prompts.
4. **Finalization Gate**: before saving a polished HTML shotlist or final prompt batch, summarize style + asset assumptions + shot count.

Skip gates when the user explicitly says to proceed, when producing a rough draft, or when only making a small natural-language revision.

## Image Asset Generation Step

Image assets are planned after Entity Registry and before final video prompts.

Asset planning order:

1. **Identity assets**: hero portrait, full body, four-view sheet when identity consistency matters.
2. **Costume / prop structure**: clothing, armor, handheld props, symbolic objects.
3. **Environment plates**: empty scene, foreground/midground/background depth, light direction.
4. **First frames**: one per high-risk clip or key transition.
5. **End frames**: only when endpoint precision or match cut matters.
6. **Storyboard / blocking sheets**: for complex group blocking, routes, stunts, or camera path.

Before generating assets, ask the Asset Gate question unless the user already provided explicit instructions. If the user says to skip assets, continue with a lower-control draft and label the risk.

Asset prompts must use the Visual Bible and Reference Role Manifest. Do not create a four-view sheet and then treat it as a final video composition.

### Asset Generation Options

At the Asset Gate, offer these modes when the user's intent is not already clear:

- **generate_with_refs**: generate image files now. If reference images exist, attach the correct references automatically according to the Reference Role Manifest.
- **prompt_only**: output all image asset prompts in a clean table/list so the user can generate them manually in another platform. Do not call any image API.
- **reuse_existing**: do not generate new images; bind existing user-provided images to roles and continue.
- **skip_assets**: continue with a lower-control draft and state which risks increase.

When using `generate_with_refs`, never attach every image to every generation. Attach only the role-correct inputs:

- Character identity asset: use existing `identity` / face / full-body references if provided.
- Costume or prop asset: use `costume_structure` / prop references; do not transfer unrelated faces or backgrounds.
- Environment plate: use `environment` / style references; do not transfer accidental people.
- First frame: combine the required character identity reference + environment reference + blocking/storyboard reference when available.
- End frame: use first frame or storyboard order only when endpoint continuity matters.
- Storyboard/blocking sheet: use environment and action/blocking references; identity can be simplified unless identity must be preserved.

When using `prompt_only`, still include the intended reference inputs per asset, for example:

```text
asset_id: first_frame_03a
mode: prompt_only
intended_refs: Image1 identity, Image2 environment, Image4 storyboard_order
prompt: [...]
negative: [...]
notes: 用户手动生成时，请把 Image1 作为人物身份参考、Image2 作为场景参考、Image4 作为动作顺序参考。
```

Do not expose API keys, raw endpoints, or provider secrets in asset prompts, HTML shotlists, or user-facing production tables.

## Shotlist HTML Mode

Use this mode when the user asks for a shotlist, prompt list, shooting checklist, HTML deliverable, batch prompts, Seedance/Higgsfield generation plan, or asks to revise an existing shotlist.

This mode borrows the useful delivery pattern of a director's shotlist, but keeps the V3 quality pipeline. Do not skip Visual Bible, Entity Registry, Reference Role Manifest, Shot Ready Gate, RAG routing, transitions, or retake discipline just because the final artifact is HTML.

### Shotlist Workflow

1. **Read as a director, not a transcriber**: find scene turns, emotional beats, reveals, entrances, exits, action peaks, and breath points.
2. **Build internal continuity state**: character appearance, costume, props, wounds/wetness/dirt, emotional carryover, location/time/weather, and previous endpoint.
3. **Number scenes**: one scene is one location/beat unit. Keep stable scene numbers on revision.
4. **Chunk prompts**: each prompt is one focused video moment. For Seedance/Higgsfield copy blocks, target `15s` by default unless the platform/user asks for shorter; long scenes split as `3a`, `3b`, `3c`. The V3 quality gate still allows `4-15s`.
5. **Route each chunk**: classify the shot need with `references/video-rag/00-routing.md`, then load at most two matching shot templates plus `01-video-prompt-schema.md`.
6. **Write final structured prompt**: use the mandatory V3 section order. Do not downgrade to generic `CUT 1 / CUT 2 / CUT 3` prose unless the user explicitly asks for the old compact format.
7. **Add continuity and transition anchors**: every prompt must include start pose, action path, endpoint pose, previous/next connection, and a named sound/object/gaze/light/occlusion/action bridge or independent cut reason.
8. **Package as HTML if requested**: produce one self-contained HTML file with inline CSS/JS, a global style/visual-bible block, scene checkboxes, prompt labels, and copy buttons.

### HTML Deliverable Requirements

- Save user-facing HTML to the current task's `outputs/` directory when available; otherwise use a clearly named `outputs/shotlist.html` under the current working directory.
- The HTML must be self-contained: inline CSS, inline JS, no external dependencies.
- Include a title, project-level style/visual-bible block, optional reference role manifest summary, and a scene list.
- Each scene has one checkbox even if it contains multiple prompts. Persist checkbox state in `localStorage` with stable keys such as `shotlist-scene-3-done`.
- Each prompt block has a copy button. The copied text must be the full generation prompt, including any required project style/visual-bible text needed for standalone use.
- Label prompt chunks clearly: `Prompt 3a · 15s`, `Prompt 3b · 15s`, or the actual duration if not 15 seconds.
- Keep visible planning text concise. The copy block is for generation; surrounding HTML is for tracking and review.
- On revision, regenerate the same HTML artifact with changes applied. Preserve stable scene numbers and prompt IDs where possible so checkbox progress survives.
- Do not include legacy runtime/vendor instructions such as `Tell Claude what to revise` or hard-coded `/mnt/user-data/outputs/` paths.

### Language Policy For Shotlists

- User-facing analysis, tables, and production notes default to Chinese when the user is working in Chinese.
- Final generation prompts default to Chinese under V3. If the target platform or user specifically needs English prompts, keep the HTML interface/notes Chinese and make the copyable generation prompt English.
- If bilingual output is useful, prefer Chinese planning text plus one final copyable platform prompt, not two competing prompt versions.

## Natural Language Revision

Support natural-language edits without requiring the user to name internal fields. Treat comments like "第三场慢一点", "整体更压抑", "女主不要哭得太明显", "换成赛博风", "这个镜头别推近", or "前两条合并" as valid revision commands.

Revision workflow:

1. **Parse intent**: identify target scope (`global style`, `scene`, `prompt chunk`, `character`, `camera`, `performance`, `sound`, `transition`, `negative prompt`, `HTML layout`).
2. **Map to controlled fields**: update `style_contract`, Visual Bible, Entity Registry, reference roles, shot duration/chunking, prompt sections, or HTML labels as needed.
3. **Preserve stability**: keep scene numbers, prompt IDs, character identity, costume continuity, and reference roles stable unless the user explicitly asks to change them.
4. **Rewrite affected parts only**: regenerate only the impacted scenes/prompts/HTML sections when possible; for a saved HTML deliverable, re-render the full file with stable IDs so checkbox progress can persist.
5. **Explain the delta briefly**: state what changed in plain language, then provide or save the revised artifact.

When the edit is ambiguous but low-risk, infer the likely meaning and proceed. Ask a clarification only when two interpretations would produce incompatible outputs, for example "更商业" could mean product-ad polish or faster TikTok pacing.

Natural-language revision must still obey V3 quality gates: one main camera movement, explicit endpoint, transition anchor, face/identity protection, sound control, and negative prompts.

## Video RAG Routing

When writing video prompts, always route by shot need before composing. Read `references/video-rag/00-routing.md`, then load only the matching template(s):

- Establishing, empty location, geography, where the scene starts: `establishing-location-shot.md`
- Single character emotion, stillness, realization, close-up reaction: `single-character-performance-shot.md`
- Dialogue, listening, shot/reverse-shot, two-person tension: `dialogue-reaction-shot.md`
- Object insert, clue, hand action, prop reveal: `object-insert-shot.md`
- Generic physical action, crossing space, reaching, falling, fighting beat: `action-process-shot.md`
- Generic motivated transition, match cut, sound bridge, gaze bridge: `generic-transition-shot.md`
- Character enters, walks out, stands, poses, holds ground: `standing-character-entrance.md`
- Door, barrier, separation, trapped character, gaze through opening: `door-separation-shot.md`
- Flood, magic, large-scale water, extreme wide VFX: `water-vfx-wide-shot.md`
- Support, protect, carry, pull back, two-person contact: `two-character-support-shot.md`
- Monks, extras, formation, crowd, blocking: `group-blocking-shot.md`
- Sleeve wipe, object wipe, mist/water occlusion transition: `sleeve-occlusion-transition.md`
- Generic final assembly and mandatory fields: `01-video-prompt-schema.md`
- Reference image role rules: `02-reference-image-roles.md`
- First-frame histogram / face integrity rules: `09-first-frame-histogram-face-integrity.md`
- If a shot fails: `08-retake-diagnosis.md`

Do not use embedding for this skill. Use deterministic keyword/tag routing first. If more than one template applies, combine at most two shot templates plus the generic schema.

## Mandatory Video Prompt Structure

Every final video prompt must include these sections, in this order:

```text
## 参考图使用
[Image1 / @tag role, locked fields, forbidden transfer]

## 风格提示词
[project visual bible, medium, palette, skin/material rules]

## 首帧直方图 / 色板约束
[histogram target, palette, black point, highlight roll-off, what not to over-apply to faces]

## 人物状态
[emotional state, intent, body status, what must remain true]

## 动作提示词
[00:00-00:02 ...; 00:02-00:05 ...; endpoint pose]
[explicitly forbid likely bad actions, e.g. 不要坐下/不要跪下/不要离开画面]

## 表演 / 微表情
[eyes, brows, mouth, breathing, pause, gaze direction]

## 脸部稳定 / 身份保护
[face shape, symmetry, no distortion, no compression, no shadow crushing on face]

## 运镜镜头提示词
[shot size, lens feel, camera height, one main movement, frame boundaries]

## 光影材质提示词
[light direction, contrast, black point, material physics]

## 音效提示词
[dialogue/no dialogue, ambience, object sounds, silence, forbidden weird audio]

## 转场锚点
[sound/object/gaze/light/occlusion/action bridge]

## 负面提示词
[motion, identity, material, camera, sound, text, watermark, modern-object negatives]
```

## Hard Rules For Video Generation

- One clip = one focused moment, one main camera movement.
- Write exact start pose, action path, and endpoint pose.
- For walking/entrance shots, state whether the character remains standing; forbid sitting/kneeling when inappropriate.
- For multi-character shots, bind positions: foreground/background, left/right, inside/outside, high/low.
- For reference images, say exactly what transfers and what must not transfer.
- If a first frame has histogram analysis, include it as a separate `首帧直方图 / 色板约束` section, not mixed into action text.
- Histogram constraints must not override face integrity: keep face anatomy stable, symmetric, natural, and readable even in low-key lighting.
- If the platform can generate sound, explicitly control sound; write `无台词，无唱腔，无锣鼓，无夸张音效` when needed.
- Avoid abstract stage terms in final prompts unless the desired medium is stage recording.
- No vague “smooth transition.” Name the visible/audible bridge.

## References

Load only when needed:

- `references/video-rag/00-routing.md`: deterministic routing table.
- `references/video-rag/01-video-prompt-schema.md`: final prompt schema.
- `references/video-rag/02-reference-image-roles.md`: image role and transfer rules.
- `references/video-rag/03-action-microexpression.md`: action and performance detail rules.
- `references/video-rag/04-camera-language.md`: camera vocabulary and movement constraints.
- `references/video-rag/05-lighting-color-material.md`: histogram, lighting, material rules.
- `references/video-rag/06-sound-negative-rules.md`: sound and negative prompt rules.
- `references/video-rag/07-transition-builder.md`: transition anchors.
- `references/video-rag/08-retake-diagnosis.md`: failure-to-fix rules.
- `references/video-rag/09-first-frame-histogram-face-integrity.md`: first-frame histogram conversion and face distortion prevention.
- `references/video-rag/examples/*.md`: shot-type templates.
- V2 inherited references in `references/` remain available for platform notes, quality gates, and research.
