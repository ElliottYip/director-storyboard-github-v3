# Retake Diagnosis

Do not reroll blindly. Change one variable per retake.

## Failure Map

| Failure | Likely cause | Next prompt change |
|---|---|---|
| Character sits/kneels unexpectedly | action too abstract, no forbidden action | add exact foot path, endpoint, "全程站立，绝对不要坐下/跪下" |
| Weird camera | camera underspecified | add shot size, camera height, one main movement, camera negatives |
| Weird sound | audio unspecified or symbolic words | add explicit sound block and forbid auto BGM/锣鼓/怪音效 |
| Stage look when real set wanted | stage terms remain | remove 幕前/牌楼亮相/台侧; add real location and material |
| Identity drift | refs not role-bound | add image role locks and forbidden transfer |
| Costume/face merges | multi-character roles vague | bind left/right, foreground/background, costume per character |
| Face slightly distorted | low-key/histogram over-applied to face, identity lock weak, camera too close/moving | add face integrity block; say histogram applies to environment, keep face readable midtones; reduce camera motion |
| Fake material | material too generic | add wet wood/stone/fabric physics and histogram |
| Rough transition | no endpoint/bridge | add endpoint hold and transition anchor |

## Retake Format

```yaml
retake:
  failed_dimension: ""
  observed_issue: ""
  keep_unchanged: []
  change_one_variable: ""
  new_instruction: ""
```
