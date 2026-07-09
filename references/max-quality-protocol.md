# Maximum Quality Protocol

Use this protocol when the user wants the best possible output, not the shortest answer. Default to this protocol for historical-drama, Seedance, KIE/GPT Image 2, Blender blocking, or multi-episode work unless the user explicitly asks for a quick draft.

## Operating Principle

Effect quality beats brevity. Do not collapse production stages into one prompt when the stages control different failure modes.

The best output is a production package:

```text
script beat
-> entity registry
-> style calibration / style contract
-> visual bible
-> asset manifest
-> role-indexed references
-> shot ready gate
-> image prompts
-> first/end frames or storyboard sheets
-> control routing / Blender blocking brief
-> video prompts
-> transition plan
-> preflight/postflight QA
-> retake log
```

## Load Order For Best Results

When working on a large episode, read these references as needed:

1. `prompt-templates.md`
2. `quality-gates.md`
3. `github-reuse-map.md`
4. `platform-notes.md`
5. `research-prompt-repair-patterns.md`
6. `research-shot-prompt-schema.md`
7. `research-platform-mapping.md`
8. `research-quality-evaluation.md`
9. Legacy references only when script formatting or Seedance manual details are needed.

## Quality Strategy

### Style Calibration

Style is not a fixed skill-level rule. Treat it as a project-level contract.

Ask before generating when a style label is broad or overloaded:

- `越剧风格`: stage recording, opera choreography in cinematic framing, or live-action drama with Yue-opera influence?
- `写实`: documentary realism, historical TV realism, or glossy commercial realism?
- `水墨`: flat ink animation, ink texture over live-action, or painterly stills?
- `电影感`: restrained cinema language, advertising hero lighting, or blockbuster spectacle?

Keep the questions short and practical. Do not ask about details that will not change prompts.

After the user answers, write:

```yaml
style_contract:
  medium: ""
  performance_convention: ""
  environment_rule: ""
  dialogue_rule: ""
  camera_rule: ""
  forbidden_interpretations: []
  reference_links_or_images: []
```

The Visual Bible must implement this style contract. Do not write project-specific style exclusions into the skill itself; keep those in the project package.

### Character Consistency

- Create role cards and identity locks before shot prompts.
- Use one identity reference per character.
- Use four-view sheets as costume/shape structure, not final composition.
- For multi-character first frames, bind positions explicitly: left / center / right, foreground / midground / background.
- For changed costume, injury, armor, wet hair, dust, or blood, create a new state asset instead of forcing the video model to invent continuity.

### Style Consistency

- Build one Visual Bible and inherit it everywhere.
- Use the same palette, black point, highlight rolloff, fog, grain, and matte skin language across all prompts.
- Treat style reference as style only.
- Never let each first frame invent its own "cinematic" style.

### Motion And Camera

- Keep one camera movement per clip.
- Split subject motion and camera motion.
- Use endpoint framing: final 1-1.5s should settle.
- Upgrade control level instead of adding more camera adjectives.
- For walking routes, group blocking, around-table movement, door/pillar/market navigation: use Blender blocking or route maps.
- For fights, embraces, grabs, falls, kneeling, horseback action, or multi-body contact: use pose/depth/canny/control video or split into smaller clips.

### Transitions

- Build adjacent clips in pairs: previous end state + current start state.
- Prefer sound bridge, object match, gaze continuation, occlusion wipe, light-color match, or action continuation.
- If no bridge exists, state why the cut is independent.
- Do not write "smooth transition" without a visible or audible mechanism.

### Image Generation

- For KIE/GPT Image 2, submit separate tasks by asset type.
- Role-index all reference images.
- Ask for exact production assets: hero view, four-view, expression/action state, scene empty, prop isolated, first frame, end frame, storyboard sheet.
- Avoid readable generated text. Use unreadable aged ink texture and add real text in post.
- If an output is blurry or AI-looking, repair lighting/material/skin/camera fields, not generic quality words.

### Retake Discipline

- Diagnose before rerolling.
- Change one variable per retake.
- Lock successful fields.
- If a failure repeats twice, upgrade control level or split the shot.

## Default Deliverable Depth

For a 5-10 minute episode, output:

- Episode beat plan.
- Clip table with duration, purpose, characters, scene, control level.
- Full Visual Bible.
- Asset manifest with required/missing status.
- Reference roles.
- Shot-ready table.
- Image prompts for all missing P0 assets.
- First-frame/storyboard prompts for each important clip.
- Seedance video prompt packages for each ready clip.
- Transition builder table.
- QA/retake plan.

If this is too much for a single answer, produce the package in ordered batches and say which batch is next. Do not skip gates silently.

## What Not To Do

- Do not produce only a pretty final prompt for complex scenes.
- Do not mix character, scene, first-frame, and style references into one vague instruction.
- Do not describe a complex walking/camera route only in prose.
- Do not use viral prompt words that create glossy AI ads.
- Do not mark a shot ready when assets are missing.
- Do not rewrite the story while improving visuals.
