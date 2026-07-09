# Director Storyboard GitHub V3

Director Storyboard GitHub V3 is a Codex skill for turning scripts, story ideas, scene notes, or reference images into structured AI-video production packages.

It is designed for Seedance, Higgsfield, Jimeng, KIE, GPT Image, and similar image/video workflows. The skill does not try to be a one-line prompt generator. It works like a small director workflow: it asks for missing high-impact choices, builds a visual bible, plans reference assets, routes each shot through Markdown templates, and then outputs structured video prompts or an editable HTML shotlist.

## What It Does

- Builds a project-level style contract and visual bible.
- Tracks characters, costumes, props, locations, and state changes.
- Plans image assets such as identity portraits, environment plates, first frames, end frames, and storyboard/blocking sheets.
- Binds every reference image to a clear role so images do not contaminate each other.
- Splits long scenes into focused 4-15 second video prompt chunks.
- Routes each shot through reusable Markdown RAG templates.
- Outputs structured prompts with action, micro-expression, camera, lighting, sound, transition anchors, and negative prompts.
- Can package prompts into a self-contained HTML shotlist with checkboxes and copy buttons.
- Supports natural-language revisions such as "make scene 3 slower" or "do not push in on this shot".

## How The Workflow Runs

1. **Style Calibration**
   The skill asks for style only when it matters. If the style is missing, it offers a compact director style menu such as realistic cinema, short-drama high emotion, historical epic, wuxia/fantasy, noir, documentary, commercial product film, or stylized animation.

2. **Visual Bible**
   The skill records palette, light direction, material rules, skin texture, camera texture, and forbidden looks.

3. **Entity Registry**
   The skill tracks characters, scenes, props, costumes, physical state, emotional state, and continuity.

4. **Asset Manifest**
   The skill decides what image assets are useful: identity images, costume/prop references, environment plates, first frames, end frames, and storyboard/blocking sheets.

5. **Asset Gate**
   Before generating image assets, the skill asks which mode you want:

   - `generate_with_refs`: generate image files now and attach the right reference images automatically.
   - `prompt_only`: export all image prompts so the user can generate them manually elsewhere.
   - `reuse_existing`: use only the images already provided.
   - `skip_assets`: continue as a lower-control draft.

6. **Reference Role Manifest**
   Each image gets one primary role, such as `identity`, `costume_structure`, `environment`, `first_frame`, `storyboard_order`, `blocking_route`, `pose_depth_canny`, `style`, or `audio`.

7. **Shot Ready Gate**
   The skill does not pretend a prompt is final if required references are missing. It can still make a draft, but it labels the risk clearly.

8. **Video RAG Routing**
   Each shot is classified by need. The skill loads the generic prompt schema and one or two shot templates from `references/video-rag/`.

9. **Structured Video Prompt**
   Each final prompt follows the same readable structure: reference image use, style, first-frame histogram, character state, timed action, performance, face stability, camera, lighting/material, sound, transition anchor, and negative prompt.

10. **Shotlist Assembly**
   If requested, the skill packages prompts into an editable HTML shotlist. Each scene has one checkbox. Each prompt has a copy button.

11. **Revision**
   Natural-language changes are mapped back to the controlled fields. Scene numbers and prompt IDs are preserved when possible.

## RAG Template Library

The RAG layer lives in `references/video-rag/`.

General rules:

- `00-routing.md`: keyword routing table.
- `01-video-prompt-schema.md`: final prompt structure.
- `02-reference-image-roles.md`: image role and transfer rules.
- `03-action-microexpression.md`: action path and performance detail.
- `04-camera-language.md`: camera vocabulary and movement limits.
- `05-lighting-color-material.md`: color, histogram, materials, skin.
- `06-sound-negative-rules.md`: sound control and negative prompts.
- `07-transition-builder.md`: transition anchors.
- `08-retake-diagnosis.md`: retake diagnosis and one-variable fixes.
- `09-first-frame-histogram-face-integrity.md`: first-frame color and face protection.

Generic shot templates:

- `establishing-location-shot.md`: location, empty plate, geography.
- `single-character-performance-shot.md`: one character, reaction, close-up, micro-expression.
- `dialogue-reaction-shot.md`: dialogue, listening, tension, shot/reverse-shot.
- `object-insert-shot.md`: props, hands, clues, product/object detail.
- `action-process-shot.md`: running, grabbing, pushing, falling, fighting beats.
- `generic-transition-shot.md`: sound bridge, gaze bridge, object bridge, match cut.

Specialized shot templates:

- `standing-character-entrance.md`: entrance, standing reveal, holding ground.
- `door-separation-shot.md`: doors, barriers, separation, gaze through an opening.
- `water-vfx-wide-shot.md`: flood, water, large-scale VFX.
- `two-character-support-shot.md`: support, protect, carry, physical contact.
- `group-blocking-shot.md`: extras, formations, crowd blocking.
- `sleeve-occlusion-transition.md`: sleeve, mist, water, or object occlusion transitions.

## Image Assets And References

When references exist, the skill automatically plans how to use them. It should not attach every image to every generation.

Examples:

- A character identity asset uses identity or full-body references.
- A costume asset uses costume or prop references.
- An environment plate uses environment and style references.
- A first frame can combine identity, environment, and storyboard/blocking references.
- An end frame uses first-frame or storyboard references only when endpoint continuity matters.

If you choose `prompt_only`, the skill outputs the intended reference inputs next to each asset prompt so users can generate the images manually in any platform.

## Environment Variables

This repository does not include real API keys.

Copy `.env.example` to `.env` only in your private local environment and fill in the providers you actually use. Do not commit `.env`.

Most users can start with `prompt_only` mode and do not need any API key.

## Installation

Manual installation:

1. Copy this folder into your Codex skills directory.
2. Keep the folder name as `director-storyboard-github-v3`.
3. Restart or refresh Codex if needed.
4. Invoke the skill with `$director-storyboard-github-v3`.

Example request:

```text
$director-storyboard-github-v3
把这个故事拆成 15 秒一条的 Seedance 分镜提示词，先只导出图片资产提示词，不生成图片。
```

## Safety And Privacy

- Do not commit `.env`.
- Do not commit real API keys, cookies, private endpoints, customer assets, or private reference images.
- Use `.env.example` to show users which variables they may configure.
- Keep generated media out of the repository unless it is intentionally licensed for public use.
- The skill instructions intentionally say not to expose API keys, raw endpoints, or provider secrets in prompts or HTML shotlists.

## License

MIT. See `LICENSE`.
