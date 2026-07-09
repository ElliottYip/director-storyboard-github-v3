# Quality Gates

Use these gates before and after generation. Do not skip them for long-form short-drama work.

## Clip Preflight

```text
[ ] Duration is 4-15s and serves one focused moment.
[ ] Audio Ledger exists; dialogue + pauses fit inside duration.
[ ] Reference roles are declared for every image/video/audio input.
[ ] Control level is declared.
[ ] Required control assets exist or are listed as missing.
[ ] Character count is manageable; multi-person dialogue is split when needed.
[ ] Only one main camera movement.
[ ] Complex path has Blender blocking / route / pose-depth-canny / storyboard sheet.
[ ] Timeline has one event per beat.
[ ] Final beat has cuttable endpoint.
[ ] previous_end_state, current_start_state, transition_anchor, or independent cut reason is present.
[ ] Visual Bible is inherited unchanged.
[ ] Matte/no-oily/no-plastic skin constraints are present.
[ ] No readable random text; signs are unreadable aged ink unless post text is planned.
```

## Image Asset Preflight

```text
[ ] Purpose is explicit: character_asset / scene_asset / prop_asset / first_frame / end_frame / storyboard_sheet.
[ ] Reference images are role-indexed.
[ ] identity_lock lists face, age, hair, build, costume state, must-not-change fields.
[ ] This generation changes only one intended variable.
[ ] Visual Bible is inherited.
[ ] First frame has movement space and clear foreground/midground/background.
[ ] Four-view sheets are not used as final video composition.
[ ] Text is avoided or converted to unreadable ink texture.
```

## Postflight Review

Score each generated clip/image as pass/fail:

```text
[ ] Character face, hair, body, costume stable.
[ ] No duplicated bodies or wrong character blending.
[ ] Scene layout and light direction stable.
[ ] Event occurs in the correct time window.
[ ] It connects to previous shot: positions, props, lighting, emotion, sound.
[ ] Camera performs only the planned movement.
[ ] Path shot has no teleporting, flying, wall/table/pillar collision, or route marks.
[ ] Action has start, process, contact/change, endpoint.
[ ] Hands, props, cloth, dust, fire, water obey basic physical logic.
[ ] Skin is matte; no oily/waxy/plastic face.
[ ] No garbled text, subtitles, watermark, logo.
[ ] Final 1-1.5s is cuttable.
[ ] Sound and mouth movement are synchronized.
[ ] Clip advances story or relationship.
```

## Retake Decision

```text
Minor color mismatch / small flicker / non-key background issue:
  Fix in post; do not reroll by default.

Wrong identity / duplicate body / prop deformation / wall collision:
  Rewrite or upgrade control level.

Same systematic error appears twice:
  Stop rerolling; diagnose and change workflow.

Every retake:
  Change one variable only.
```

## Failure Dimensions

Use these labels in `retake.failed_dimension`:

- `identity`
- `style_alignment`
- `reference_role_conflict`
- `composition`
- `motion_order`
- `camera_control`
- `spatial_blocking`
- `transition`
- `physics`
- `lip_sync_audio`
- `text_artifact`
- `skin_material`
- `platform_limit`

## Repair Priority

1. Identity and subject integrity.
2. Temporal stability and flicker.
3. Camera logic and endpoint.
4. Physical action and contact points.
5. Visual realism: matte skin, cloth, grain, low-key palette.
6. Story semantics and event order.
7. Editing usefulness: endpoint and transition anchor.
