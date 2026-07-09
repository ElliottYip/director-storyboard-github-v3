# Prompt Templates

Use these templates when the user asks for production prompts, first frames, storyboard sheets, or video clips.

## Image Prompt Schema

Use for `character_asset`, `scene_asset`, `prop_asset`, `first_frame`, `end_frame`, and `storyboard_sheet`.

```json
{
  "meta": {
    "purpose": "character_asset | scene_asset | prop_asset | first_frame | end_frame | storyboard_sheet",
    "aspect_ratio": "16:9",
    "style": "photorealistic Eastern Han historical TV drama, low-key desaturated green-gray palette"
  },
  "reference_usage": [
    {
      "image": "Image1",
      "role": "identity | environment | style | first_frame | storyboard_order",
      "target": "",
      "transfer_only": [],
      "do_not_transfer": []
    }
  ],
  "subject": {
    "identity_anchor": "",
    "age_build_face": "",
    "wardrobe_hair": "",
    "pose_expression": ""
  },
  "identity_lock": {
    "face_shape": "",
    "age_range": "",
    "skin_texture": "matte natural skin, subtle pores, no oily highlights",
    "hairline_hair_style": "",
    "body_type_height": "",
    "costume_state": "",
    "must_not_change": ["face identity", "age", "hairline", "body type", "costume state"]
  },
  "environment": {
    "location": "",
    "foreground": "",
    "midground": "",
    "background": "",
    "historical_materials": ["linen", "rough wood", "ceramic", "bronze", "dust"]
  },
  "lighting": {
    "histogram": "shadows 55%, midtones 35%, highlights 10%",
    "palette": ["#111614", "#2E3A35", "#5C6B63", "#94A39A", "#E2EBE0"],
    "skin": "matte, low-reflection, natural pores, no oily highlights"
  },
  "camera": {
    "shot_size": "",
    "angle": "eye_level",
    "lens_mm": 35,
    "depth_of_field": "natural cinematic depth"
  },
  "negative": [
    "over-smoothed skin",
    "waxy face",
    "plastic skin",
    "oily skin",
    "warped hands",
    "extra fingers",
    "readable random text",
    "watermark",
    "logo",
    "modern objects",
    "CGI/illustration feel"
  ]
}
```

## Character Hero Prompt

```text
Photorealistic Eastern Han historical television drama, full-body character asset on clean white background.
[Character name], [age], [height/build], [face shape], [eyes], [hairline and hairstyle], [facial hair if any],
wearing [historically grounded costume, fabric, wear state, shoes].
Neutral stance, arms relaxed, full body from head to shoes visible, no held object unless specified.
Matte natural skin with subtle pores, low-reflection face, no oily highlights.
Inherit Visual Bible: low-key desaturated green-gray realism, restrained contrast, soft highlight roll-off.
No text, no watermark, no logo, no modern elements, no waxy/plastic skin, no beauty filter.
```

## Four-View Prompt

```text
Create a production character reference sheet for [Character].
Left third: large face close-up.
Right two-thirds: full-body front view, side view, and back view of the same person.
White background, consistent face, hair, body type, costume structure, shoes, fabric texture.
Neutral expression, no props, no scene background.
Photorealistic Eastern Han historical TV drama, matte skin, no oily or waxy face.
No text labels, no watermark, no random writing.
```

## Scene Asset Prompt

```text
Photorealistic Eastern Han historical TV drama environment asset, no people.
[Location name and function].
Foreground: [objects/materials].
Midground: [walkable space, doors, table, pillars, route].
Background: [architecture, haze, crowd only if blurred/non-identifiable].
Light direction: [direction], low-key desaturated green-gray volumetric haze, lifted black point, soft warm highlights.
Materials: coarse wood, clay, linen, bronze, old paper, dust in air.
No modern objects, no readable text, no watermark, no logo, no people.
```

## First Frame Prompt

```text
First frame for [clip_id], 16:9 horizontal, photorealistic Eastern Han historical TV drama.
Reference roles:
- [Image1] identity only for [character].
- [Image2] environment only for [scene].
- [Image3] style only if present.
Composition: [shot size, lens, angle, foreground/midground/background].
Character blocking: [left/center/right positions], [gaze direction], [start pose], [movement space].
Lighting: inherit Visual Bible exactly.
Skin/material: matte skin, no oily highlights, natural fabric texture.
No readable text, no subtitles, no watermark, no logo.
```

## Storyboard Sheet Prompt

```text
Create a 4-panel production storyboard sheet for [clip_id], 16:9 horizontal contact sheet.
Photorealistic rough film still storyboard, not a poster, no decorative title.
All panels inherit the same Visual Bible and character references.

Panel 1: [shot size/camera angle], [starting pose/action].
Panel 2: [single next action node].
Panel 3: [single next action node].
Panel 4: [ending pose/state, cuttable endpoint].

Maintain the same characters, costumes, scene layout, lighting direction, and palette across all panels.
No readable generated text, no subtitles, no watermark, no logo, no oily/waxy/plastic skin.
```

## Video Prompt Package

```text
## Audio Ledger
[speaker / line / emotion / mouth visibility / seconds]
[ambient/object sounds]
[audio bridge to next clip]

## References
@Image1 role: identity only for [character]; do not transfer background or pose unless specified.
@Image2 role: environment only for [scene]; do not transfer faces.
@Image3 role: first-frame lock for [clip_id].
@Image4 role: storyboard order for [clip_id].

## Visual Bible
[copy shared Visual Bible]

## Control Level
[prompt_only | first_frame | first_last_frame | storyboard_sheet | blender_blocking | pose_depth_canny]
[reason and required assets]

## Timeline
00:00-00:03 [one action node, one camera movement]
00:03-00:07 [one action node]
00:07-00:10 [ending state and final hold]

## Camera
Angle: [eye_level / low_angle / high_angle / over_the_shoulder / pov / overhead]
Movement: [static / slow_push_in / stable_tracking / truck_left / pan_left etc.]

## Physical Details
[cloth, dust, fire, paper, water, wood, contact point, gravity, inertia]

## Transition Anchor
[sound/object/gaze/light/occlusion/action bridge or independent cut reason]

## Sound Design
[ambient, object, dialogue, breath; avoid random music unless requested]

## Negatives
No subtitles, no readable text, no watermark, no logo, no face drift, no costume drift,
no random camera movement, no oily skin, no waxy face, no plastic skin.
```

## Blender Blocking Brief

Use when `control_level: blender_blocking`.

```yaml
blender_blocking_brief:
  scene_scale: ""
  fixed_objects: ["door", "table", "pillar"]
  characters:
    - name: ""
      start_position: ""
      end_position: ""
      path_notes: ""
  camera:
    start_position: ""
    end_position: ""
    height: ""
    lens_hint: ""
    axis_rule: ""
  exports_needed:
    - "top_down_route_map"
    - "clean_first_frame"
    - "optional_end_frame"
    - "optional_depth_canny_pose"
  video_prompt_scope: "follow approved path only; do not invent new route"
```

## Transition Prompt

```text
Use [start_frame] as the exact beginning and [end_frame] as the exact ending.
Bridge the two shots through [sound/object/gaze/light/occlusion/action].
Keep the same Visual Bible, matte skin, palette, scene logic, and physical direction.
Duration [4-6s]. Final 1s settles into the end frame.
No new characters, no changed costumes, no random camera orbit, no readable text.
```

## Retake Prompt

```yaml
retake:
  take_id: ""
  failed_dimension: ""
  observed_issue: ""
  keep_unchanged: []
  change_one_variable: ""
  rewrite_instruction: ""
```
