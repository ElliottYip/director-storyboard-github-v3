# Platform Notes And Evidence Boundaries

Keep official facts, third-party execution schemas, and community workflow lessons separate.

## Seedance 2.0

Use as the primary video prompt target when the user asks for Seedance.

Stable rules for this skill:

- Write clips as 4-15s segments.
- Use multimodal references with clear roles.
- Treat 15s as an upper bound, not a reason to overload action.
- Put Chinese dialogue, ambient sound, and object sounds in the prompt when needed.
- Use one main camera movement per clip.
- End with a cuttable endpoint.

Known risks to guard against:

- Cross-clip identity drift.
- Multi-person lip sync and multi-subject consistency.
- Text rendering and random subtitles.
- Detail instability, waxy/oily skin, AI polish.

## GPT Image 2 / KIE

Use for character hero views, four-views, scene assets, first frames, end frames, and storyboard sheets.

KIE is a third-party execution layer. If executing through KIE:

- Keep API keys out of skill files and output.
- Use task ids and local manifests to track outputs.
- Role-index every `input_urls` image.
- Prefer 16:9 for scene/first-frame/storyboard, 9:16 or 16:9 for character full body depending on downstream need.
- For identity-sensitive edits, change one variable per run.

## Runway

Use only when relevant to the user's platform choice or as reference-tagging inspiration.

Useful execution concepts:

- `promptImage`
- `promptText`
- `ratio`
- `duration`
- `referenceImages`
- `@tag` references

Migration rule: name references like `@LiuBei_identity`, `@Taoyuan_environment`, `@MatteSkin_style`.

## Pika / Kling / Wan Via Fal Or Other Third Parties

Treat these as third-party schemas unless official docs are verified in the current task.

Allowed wording:

- "Fal schema suggests..."
- "Third-party execution route..."
- "Use as adapter clue, not official prompt rule."

Avoid:

- Claiming Fal fields are official Pika/Kling/Wan prompt guidance.
- Copying platform fields into Seedance prompts without adaptation.

## ComfyUI / Wan / Local Workflows

Use as workflow hints for control escalation:

- First/last-frame workflow for exact endpoints.
- Fun Camera / ReCamMaster / WanMove / trajectory for camera/walking route.
- OpenPose / Depth / Canny / ControlNet / WanAnimate preprocess for complex body motion.
- Talking/lipsync workflows for dialogue-heavy closeups.

Do not claim exact node parameters until the concrete workflow JSON is read.

## Sora

Treat Sora only as historical/deprecated product-form inspiration. Use concepts like:

- Storyboard cards.
- Leaving space between storyboard cards to avoid hard cuts.
- Re-cut / remix / blend / loop as editing metaphors.

Do not write Sora as a current execution adapter.
