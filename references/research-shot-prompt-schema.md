# 分镜图片与视频 Prompt Schema 草案

状态：研究草案，尚未写入 skill。

## 图片 Prompt Schema

用于人物资产、场景资产、首帧图。核心来自 `image-prompt-layers`、Gemini Image best practices、用户给定的直方图/色板约束。

```text
Meta:
  真实东汉历史电视剧，横屏 16:9，低调 Low-Key，低饱和灰绿色体积雾，非插画，非游戏渲染。

Subject:
  [角色/场景主体]，年龄、体态、服装、发型、姿势、表情、参考图对应关系。

Scene:
  前景 / 中景 / 背景空间层次；历史时期材料；是否有人群；画面主体位置。

Lighting:
  暗部约 55%，中间调约 35%，高光约 10%；
  暗部 #111614 / #2E3A35，雾气 #5C6B63 / #94A39A，高光 #E2EBE0；
  柔和高光滚降，抬升黑位，4500K-5000K 暖白高光；
  人物皮肤低反射、哑光、无油光。

Camera:
  焦段、景别、机位、景深。人物资产默认正交/棚拍白底；首帧默认 35mm/50mm 电影镜头。

Details:
  麻布、粗木、陶器、泥土、青铜、帷幕、尘粒、纸张边缘、旧化痕迹。

Negative:
  无现代元素、无字幕、无水印、无 logo、无可读乱码文字、无油光皮肤、无蜡像脸、无塑料皮肤、无过锐、无 HDR glow。
```

## 图片 JSON Schema 版本

结构化图片 prompt 库普遍使用 JSON 字段而不是散文。后续 skill 可以同时输出“人读 markdown”和“机器读 JSON”两种版本。

```json
{
  "meta": {
    "purpose": "character_asset | scene_asset | first_frame | storyboard_still",
    "aspect_ratio": "16:9",
    "style": "photorealistic Eastern Han historical TV drama, low-key, desaturated green-gray palette"
  },
  "subject": {
    "identity_anchor": "reference image id",
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
    "depth_of_field": "natural cinematic shallow depth where appropriate"
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

从 GitHub30 / YouMind / devanshug2307 等图片 prompt 库抽样得到的可迁移点：

- 写实人物 prompt 应显式写 `natural skin texture`、`no retouching`、`no beauty filter`，并在 negative 里排除 `over-smoothed skin / doll face / warped hands`。
- 环境 prompt 应列出可见前景道具和背景元素，避免模型用泛化“古代场景”填充。
- 相机字段越具体，越像真实拍摄：lens look、焦段、曝光/光源方向、是否自然景深。
- 如果需要“无文字”，不要只写 no text；道具上的文字应改为“aged ink texture / unreadable marks”。
- 高星图片 prompt 库中有大量 JSON/半 JSON 写法，最可迁移的是字段分栏，不是具体网红风格：`identity_lock`、`environment`、`lighting`、`camera`、`constraints`、`negative_prompt`。

OpenAI 官方 GPT Image prompting guide 对当前项目的迁移：

- 多图引用时必须用索引/名称说明每张图的作用，避免模型把场景图里的人、人物图里的白底、四视图里的多个角度互相污染。
- 身份敏感编辑适合小步迭代：一次只改角度、表情、服装状态、光线中的一个变量。
- 复杂图像先拆成角色资产、场景资产、首帧、storyboard sheet，再组合；不要一条 prompt 同时要求人物表、场景、动作、文字和多格分镜。
- 如果需要可读文字，先判断是否真的必须在图像模型阶段生成；历史剧榜文/牌匾默认用不可读墨迹纹理，后期叠字更稳。

Runway API docs 对参考图命名的迁移：

- 图片任务的 `referenceImages` 可以用 `tag`，并在 `promptText` 中用 `@tag` 引用；这说明参考图不应只是附件列表，而应有清晰角色。
- 对三国项目，建议统一命名为 `@LiuBei_identity`、`@GuanYu_identity`、`@ZhangFei_identity`、`@Tavern_environment`、`@MatteSkinLook_style`。
- 每个 tag 后必须写用途：identity only / environment only / lighting only / costume texture only / first-frame composition only。
- 未打 tag 的图容易被模型当成泛参考；用于人物一致性时不要混入白底四视图、场景图和不同角色图。

## Storyboard-First 两阶段控制

来自 AGI-Ruby/storyboard-to-seedance-video 的高价值思路：不要让视频 prompt 同时承担“设计分镜”和“执行分镜”两件事。先生成 storyboard sheet，再把 storyboard 当作关键帧链喂给视频模型。

```text
Reference A: 角色参考图，用于锁脸、体态、服装、发型。
Reference B: storyboard / shot logic sheet，用于锁 shot order、framing、body movement、movement direction、camera rhythm、ending state。
```

图片阶段输出 storyboard sheet 时，应要求：

- panel count：例如 3 panels / 4 panels，而不是一张复杂拼贴。
- 每格只表达一个动作节点：起势、反应、接触、收尾。
- 标注景别、机位、主动作、终点姿态。
- 环境保持克制，避免每格都塞满新场景。
- 视觉粗糙度可以是“production storyboard still”，不必追求每格成片级，但角色和光影要继承 visual bible。

视频阶段使用 storyboard sheet 时，应要求：

```text
严格按 storyboard 顺序执行，不重新发明镜头。
角色身份以角色参考图为准；镜头顺序和动作递进以 storyboard 图为准。
把 storyboard 转译成连续实拍质感运动，保持最后一格的 ending state。
```

## 视频 Prompt Schema

用于 Seedance 15 秒以内镜头。核心来自 Seedance masterclass、Kling 公式、Veo 官方字段、VBench 质量维度。

```text
Audio ledger:
  本 clip 内所有对白/旁白/内心 OS，speaker、内容、预计秒数、是否张嘴。
  音频是硬约束：镜头时长必须大于等于对白时长 + 停顿缓冲。

Reference usage:
  使用哪些人物图/场景图/首帧图/storyboard sheet；每个角色对应哪个图；不要把四视图当多人。
  角色图锁身份，storyboard sheet 锁镜头顺序，首帧图锁当前 clip 的构图和风格。
  若平台支持 @tag 或 Image1/Image2，引文必须与资产表一致，不临时改名。

Visual bible:
  东汉历史电视剧真实风格；低调灰绿色直方图；哑光皮肤；柔和高光滚降；轻微胶片颗粒。

Characters:
  exactly N main characters: [角色名]。每人只给一个主动作或反应。

Timeline:
  [00:00-00:05] opening：建立人物位置/冲突/动作起点
  [00:05-00:10] middle：承载对白、互动或物理推进
  [00:10-00:15] ending：动作收住 / 声音桥 / 可剪辑静态点

Camera angle:
  景别与机位，如 medium shot / low angle / over-the-shoulder。

Camera movement:
  只允许一个主运镜，如 fixed camera / slow push-in / stable tracking。

Physical details:
  布料、尘土、火光、酒水、纸张、雾气的物理因果。

Audio:
  环境声、物体声、对白/呼吸/停顿；不要泛写 sound effects。

Transition anchor:
  相邻同空间 clip 必须选一个：action continuation / gaze direction / sound bridge / object match / light-color match / occlusion blackout。
  如果没有衔接锚，优先合并为同一个 10-15s clip。

Continuity:
  角色面部、服装、发型一致；画面色调一致；镜头结尾便于剪辑。

Negative:
  无字幕、无文字、无水印、无 logo、无分身、无突然变焦、无剧烈摇晃、无油光皮肤、无塑料皮肤。
```

## Seedance 六段式输出模板

来自 seedance-director 的可迁移结构，适合后续 skill 直接输出。

```text
## Audio Ledger
刘备@低声说：...（预计 3s）
张飞@不说话，只呼吸加重（不张嘴）

## Characters + References
@图片1 作为刘备角色参考：...
@图片2 作为张飞角色参考：...
@图片3 作为涿县酒肆场景参考：...
@图片4 作为本段首帧/分镜参考：...

## Background
东汉末年，涿县酒肆外，黄巾乱起前的压抑民间气氛...

## Shot Timeline
00:00-00:04 ...
00:04-00:09 ...
00:09-00:15 ...

## Transition Anchor
本段结尾以 [道具/视线/声音/遮挡] 连接下一段；最后 1.5 秒动作收住。

## Sound Design
BGM / ambient / object sound / dialogue 分开写。

## Style Directives
继承 Visual Bible：低调灰绿色、体积雾、抬升黑位、哑光皮肤、真实历史剧摄影。

## Negatives
No subtitles, no readable text, no watermark, no logo, no oily skin, no waxy face, no sudden camera movement.
```

## 八要素 Shot Card

来自 GongLingRui/ai-video-generation 的可迁移结构。它适合放在 storyboard 表格中，作为每个 shot 的最小完备描述。与上面的六段式不同，八要素更适合“单镜头卡片”，六段式更适合“15 秒视频 prompt”。

```json
{
  "shot_id": "EP01-S003",
  "duration_sec": 5,
  "subject": "刘备站在榜文前，目光停住",
  "composition": "medium close-up, eye-level, Liu Bei on right third, imperial notice blurred in foreground",
  "lighting": "low-key desaturated green-gray, lifted black point, warm paper highlight, matte skin",
  "background": "涿县街口，远处百姓模糊围观，木柱、尘土、旧纸",
  "action_movement": "刘备微微吸气，眼神从榜文移向远处，衣袖被风轻推",
  "text_overlay": "none; notice text appears as aged ink texture, unreadable",
  "transition_detail": "final 1.5s settles on still composition; paper edge moves left-to-right for match cut",
  "audio": "distant crowd murmur, paper flutter, low drum pulse"
}
```

与当前三国项目的关系：

- `subject` 不写泛化“古代男子”，必须写角色名和当前状态。
- `composition` 负责避免每个镜头都是同一种中景。
- `transition_detail` 是解决拼接生硬的固定字段。
- `text_overlay` 默认 none；榜文/牌匾只做不可读纹理，真文字后期叠。

## Shotlist Pattern 规则

来自 shotlist-forge：把一个 concept 展开成 ordered shot list，而不是单 prompt。可迁移字段：

```text
continuity fields: style / lighting / mood / lens / aspect
beats: 每个 beat 对应一个 shot
pattern: narrative / reveal / montage / ad
duration: 总时长按 shot 分配整秒
```

对历史剧推荐 pattern：

| 场景 | Pattern | 镜头节奏 |
|---|---|---|
| 张榜/乱世交代 | reveal | tight detail -> medium reaction -> wider environment |
| 酒肆初遇 | narrative | establishing -> medium dialogue -> close reaction -> two-shot |
| 桃园结义 | narrative/reveal | wide location -> hands/cups detail -> faces -> final wide oath |
| 战场首功 | montage | preparation -> impact -> reaction -> aftermath，但每个动作拆短 |

## 官方平台约束补丁

### 短视频单场景原则

```text
每条 4-15 秒视频只服务一个 focused moment。
如果剧本里有 A→B→C 三个不同事件，拆成三个 clip，而不是塞入同一条 prompt。
```

### I2V motion-only 原则

```text
源图已经提供人物、场景、构图、灯光、风格。
视频 prompt 只补：相机运动、主体微动作、环境变化、声音。
不要重新描述服装、建筑、色调，除非是在修正源图缺陷。
```

### Camera 枚举优先

```text
Camera angle: eye_level / low_angle / high_angle / overhead / over_the_shoulder / pov
Camera movement: static / push_in / pull_out / pan_left / pan_right / tilt_up / tilt_down /
truck_left / truck_right / orbit_left / orbit_right / handheld / aerial_drone
```

历史剧默认安全组合：

```text
Dialogue: eye_level or over_the_shoulder + static or slow_push_in
Walking: eye_level or low_angle + stable_tracking/truck
Reveal: wide_establishing + tilt_up or slow_push_in
Emotion: close_up + static or very_slow_push_in
Complex geography: overhead/aerial only for establishing shot
```

## 镜头转场模板

```text
镜头结尾停在可剪辑的静态动作点，人物动作自然收住，环境声淡出，
画面保留同一低调灰绿色调和柔和高光滚降，便于下一镜硬切。
```

```text
用物体动作衔接下一镜：[道具/环境元素] 在风中轻动，扫过画面边缘；
下一镜以同方向运动的 [另一个道具/环境元素] 开始。
```

## Multi-Beat Timing Template

来自 Seedance prompt 库、Higgsfield 技能集、广告视频 prompt 库和官方 4-15s 限制的共同结论：短 clip 要写成多拍点，而不是长散文。

```yaml
clip_timing:
  duration: 8s
  aspect: 16:9
  beat_1:
    time: 0-2s
    action: "张飞的手掌悬在木案上方，眼神压住怒气"
    camera: "locked medium close-up, slight handheld breath"
    sound: "酒肆远处杂声，掌风压低"
  beat_2:
    time: 2-4s
    action: "手掌落下，木案震动，酒碗晃出一圈水纹"
    camera: "subtle push-in only"
    sound: "one heavy table hit, bowl rattle"
  beat_3:
    time: 4-7s
    action: "刘备抬眼，不退，右手扶住草席"
    camera: "hold the same axis, no orbit"
    sound: "room tone drops, breath visible"
  hold:
    time: 7-8s
    endpoint: "all motion settles into a cuttable still frame"
fidelity_guard:
  - "same face, same robe, same prop positions"
  - "no deformation, no drift, no temporal flicker"
  - "matte low-reflection skin, no oily/waxy face"
```

迁移规则：

- 每 2-4 秒只推进一个动作节点。
- 每个 beat 只允许一个主运镜，不能同时 dolly/orbit/crane/FPV。
- `fidelity_guard` 应具体到人物、道具、材质、皮肤、色彩，不写泛泛的 `high quality`。
- 历史剧不能照搬广告库的 hero product 语气；保留 timing/guard/sound 结构即可。
