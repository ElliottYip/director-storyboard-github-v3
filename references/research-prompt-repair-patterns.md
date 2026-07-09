# Prompt 修复模式库

状态：进行中。后续可转成 `director-storyboard-github` 的失败诊断和自动重写模块。

## 修复模板

### 1. 人物油光 / 塑料皮肤

症状：额头、鼻梁、脸颊油亮；皮肤像蜡像或 3D render。

```text
Lighting: soft directional practical light, low-reflection skin, soft highlight roll-off, lifted black point, no glossy beauty lighting.
Skin: natural matte skin texture, subtle pores, slight imperfections, no over-smoothing.
Negative: oily skin, waxy face, plastic skin, HDR glow, beauty filter, fashion studio shine.
```

中文版本：

```text
人物皮肤为真实哑光质感，保留轻微毛孔和细小瑕疵；额头、鼻梁、脸颊无油亮反光；
不要美颜磨皮，不要蜡像脸，不要塑料皮肤，不要 HDR 发光。
```

### 2. 运镜怪 / 镜头乱飞

```text
Camera movement: fixed camera with a very slow push-in only.
No orbit, no whip pan, no sudden zoom, no handheld shake, no Dutch angle.
The shot ends on a stable, editable composition.
```

中文版本：

```text
本镜头只使用一种主运镜：固定机位缓慢推近。不要环绕、不要快速摇镜、不要突然变焦、
不要手持乱晃、不要倾斜地平线。镜头结尾停在稳定可剪辑构图。
```

### 3. 切换生硬

```text
End the shot on a quiet transitional object: [object] gently moves in the same direction as the next shot.
The character's motion settles before the cut. Ambient sound fades naturally.
Keep the same low-key desaturated green palette and soft highlight roll-off.
```

### 4. 动作不真实 / 没有物理因果

把“结果词”改成“接触点 + 力 + 介质 + 终点”。

```text
Bad: dust rises.
Better: His boot presses into the dry earth, the impact pushes a low puff of dust outward, then the dust drifts sideways through the doorway light.
```

```text
Bad: he drinks wine.
Better: His fingers grip the rough ceramic bowl rim, he tilts it toward his lips, the wine surface trembles once, then settles as he lowers the bowl.
```

### 5. 多人混乱 / 分身

```text
Exactly two main characters are visible: Liu Bei (reference image 1) and Zhang Fei (reference image 2).
No duplicate bodies, no clone, no extra version of either character.
Background people remain soft and indistinct.
```

### 6. 图生视频漂移

```text
Use the uploaded image only as the visual reference and first frame.
Do not redesign the character, costume, architecture, color palette, or composition.
Only animate: subtle breathing, eye movement, fabric motion, candle flicker, and the specified camera movement.
```

如果是 I2V，修复时不要把 prompt 变长重描述画面。只改 motion 字段：

```text
Motion only: the subject breathes subtly, eyes shift toward the door, sleeve fabric moves in a faint side draft,
candle flame flickers once, camera performs a very slow push-in. Preserve the source image as the first frame.
```

### 7. 文字乱码 / 字幕误触发

```text
No subtitles, no captions, no watermark, no logo, no readable text.
Any poster or notice in the background should appear as aged ink texture, not readable characters.
```

### 8. 角色状态变化导致不一致

```text
When the character changes outfit, injury state, armor, hairstyle, or age, create a new character sheet for that state.
Do not reuse the civilian reference for the armored battle scene.
```

### 9. 短 clip 内容过载

症状：模型漏掉事件、画面不完整、动作混在一起。

修复：拆成单一 focused moment。

```text
Clip 1: close-up of Liu Bei's eyes stopping on the imperial notice.
Clip 2: medium shot of Zhang Fei stepping into the tavern doorway.
Clip 3: static two-shot as the two men recognize each other's anger and restraint.
```

### 10. 对白触发画面文字

症状：画面出现字幕、乱码、对话文字。

修复：不用英文引号包裹对白；对白字段独立，画面字段明确 no text。

```text
Dialogue:
刘备低声说: 天下将乱，百姓何辜。

Negative:
No subtitles, no captions, no readable text, no floating dialogue text.
```

### 11. 风格不统一 / 像 AI 跑图

症状：同一集不同镜头色调、皮肤、雾气、镜头语言不一致；画面像游戏 CG、广告棚拍、网红写真或过度锐化。

修复：每条图片和视频 prompt 都继承同一 Visual Bible，不允许每镜头重新发明风格。

```text
Visual Bible, inherited unchanged:
Eastern Han historical TV drama, photorealistic live-action cinematography, low-key desaturated green-gray palette,
lifted black point, soft highlight roll-off, volumetric haze, restrained contrast, natural fabric texture,
matte skin with subtle pores, no glossy beauty lighting, no CGI/game render/anime/poster look.

Color / histogram:
shadows 55% with #111614 and #2E3A35, midtones 35% with #5C6B63, highlights 10% with #E2EBE0,
warm high-light bias around 4500K-5000K.
```

中文版本：

```text
全片继承统一视觉圣经：真实东汉历史电视剧摄影，低调灰绿色，低饱和，暗部占比高但黑位抬升，
高光柔和滚降，空气中有体积雾，布料、木器、泥土、陶器为真实旧化材质。
人物皮肤哑光、有轻微毛孔，不要油光，不要蜡像脸，不要游戏 CG，不要广告棚拍，不要网红写真。
```

### 12. 首帧漂亮但视频执行偏离

症状：首帧统一，但视频一动就换脸、换服装、换场景或新增无关镜头。

修复：图生视频 prompt 改成 motion-only，并明确哪些由首帧锁定。

```text
Use @图片4 as the first frame and composition lock.
Preserve the existing character identity, wardrobe, scene layout, lighting, color palette, and camera framing from the first frame.
Only animate: eye movement, breathing, sleeve fabric, candle flame, dust drift, and the specified slow push-in.
Do not introduce new camera angles, new characters, new costumes, or new background architecture.
```

### 13. 分镜之间切换生硬

症状：每段单看还行，拼起来像硬切的图库；方向、声场、动作 endpoint 不连续。

修复：为每段增加 entry/exit hook。

```text
Entry hook: begins with the same screen direction and sound texture as the previous clip.
Exit hook: the final 1.5 seconds settle into a stable composition; the main motion slows to stillness.
Match cut object: [cloth edge / candle smoke / dust / door shadow] moves left-to-right, ready to cut into the next clip.
Ambient audio: fade the tavern room tone under the final breath before cut.
```

中文版本：

```text
本段开头继承上一段的运动方向和环境声；最后 1.5 秒动作自然收住，停在可剪辑构图。
用同方向的衣角、烛烟、尘土或门影作为匹配剪辑物，避免每段像独立图库画面。
```

### 14. 续写从“计划结尾”开始，而不是从真实成片结尾开始

来源：Emily2040/seedance-2.0 的 continuation / failure-atlas 思路。

症状：上一段实际停在门口，但下一段 prompt 按原计划从院内开始；人物位置、镜头 phase、动作速度断裂。

修复：每段通过验收后记录 observed end state，下一段只从真实结束状态编译。

```text
Observed end state from accepted Clip 03:
Liu Bei stands at the tavern threshold, half profile, right hand still on the door frame.
Camera is eye-level, medium shot, slow push-in has already settled.
Dust moves left-to-right; tavern room tone is quiet.

Clip 04 must begin from this exact state.
Do not restart the action from the street. Do not repeat the completed doorway entrance.
```

### 15. 动作重复 / 已完成事件又演一遍

症状：上一段已经张榜、抬头、进门，下一段又把同一动作重来。

修复：每条 prompt 增加 completed beat exclusion。

```text
Completed beats, do not show again:
- The imperial notice has already been posted.
- Liu Bei has already read the notice.
- Zhang Fei has already entered the tavern.

Current beat only:
Zhang Fei stops beside Liu Bei, lowers his voice, and tests whether Liu Bei feels the same anger.
```

### 16. Reference 角色污染 / 风格污染

症状：用一张图做角色参考，模型把该图里的背景、服装状态、光效也带到别的镜头；或场景参考影响人物脸。

修复：每个参考图必须声明 role + transfer + ignore。

```text
@图片1 role: Liu Bei identity reference only. Transfer face shape, age, hairstyle, body type, and plain scholar robe.
Ignore the white studio background and exact pose.

@图片2 role: tavern environment reference only. Transfer wooden beams, mud floor, oil-lamp atmosphere, room layout.
Ignore any people or modern objects in the image.
```

### 17. Retake 反复乱改，越修越坏

来源：Emily2040/seedance-2.0 的 retake protocol。

症状：同一条失败后同时改镜头、改风格、改人物、改参考图，无法判断哪个变量有效。

修复：每次 retake 只改一个变量，并记录 verdict。

```text
Take 02 changed: camera only, from orbit to fixed slow push-in.
Seed: same.
Verdict: rewrite if the same camera drift appears again.
Evidence: face identity stayed correct, but camera still rotates unexpectedly after 6s.
```

操作规则：

- 一次只改一个变量：prompt clause / seed / mode / one reference。
- 同一系统性错误出现两次，停止 re-roll，进入 rewrite。
- 画面文字、轻微颜色、片头片尾不稳优先后期修，不消耗生成次数。

### 18. 运镜怪 / 像模型在乱飞

症状：画面突然旋转、推拉摇移同时发生、人物关系被运镜破坏、空间像游戏穿模。

常见根因：

- 一条 prompt 同时写了 push in、orbit、tracking、crane、FPV、handheld。
- 没有明确镜头身份和终点。
- 把复杂空间路线交给文本模型猜，而没有首帧/尾帧/路径图/Blender 预演。

修复模板：

```text
Camera: locked-off tripod, eye-level medium shot. No orbit, no crane, no handheld shake, no sudden zoom.
Only motion: a very slow push-in of less than one meter over 8 seconds.
Endpoint: camera settles into a stable medium close-up in the final 1.5 seconds.
```

复杂路径改法：

```text
Do not improvise camera movement.
Camera path is a single physical route: tavern doorway -> Liu Bei at the table -> Zhang Fei's hand on the wine bowl.
No wall penetration, no flying, no rotation, no lens roll.
If this path is important, generate Blender/Motion Control preview first; video prompt only follows the approved path.
```

### 19. AI 感 / 油光皮肤 / 蜡像脸

症状：人物脸像塑料、额头鼻梁发亮、皮肤过锐、衣服过干净，历史剧像棚拍广告。

来源补充：RealGen / RealBench 将 photorealism 拆成 artifact、realism、details、aesthetics 等评价方向；因此修复时不要只写 `photorealistic`，要把“假真实”的痕迹逐项拆开。

修复应同时放到 Lighting、Details、Negative，而不是只放 negative：

```text
Artifact control: no oily skin, no glossy forehead, no waxy face, no plastic texture, no over-sharpened HDR edges.
Realism: soft diffused side light, low-key exposure, lifted black point, gentle highlight roll-off.
Details: matte skin texture, subtle pores, dust on fabric, natural uneven cloth fibers, no beauty retouch.
Aesthetics: restrained historical TV drama lighting, not fashion studio, not CGI/game render.
```

视频 prompt 追加：

```text
Throughout the clip, skin remains matte and natural under soft diffused light; highlights roll off gently without oily shine.
```

### 20. 多人同屏口型/对白混乱

症状：嘴动和声音错位，多人说话声音串，角色 A 的台词像角色 B 说的。

官方 Seedance 2.0 发布页也把多人口型匹配列为仍需改进项，因此 prompt 侧要主动降复杂度。

修复：

```text
Only one visible speaking character in this shot.
Other characters listen silently, breathing naturally, no mouth movement.
[Speaker: Liu Bei] speaks one short line in a restrained low voice.
Background sound remains below the dialogue; no overlapping voices.
```

如果必须多人对白：

```text
Split into shot-reverse-shot.
Shot A: Liu Bei speaks, Zhang Fei listens silently.
Shot B: Zhang Fei replies, Liu Bei listens silently.
Do not generate simultaneous dialogue.
```

### 21. 官方承认的文字还原/复杂编辑风险

症状：榜文、字幕、牌匾出现乱码；编辑任务误改人物脸或背景；想去掉瑕疵却把非编辑区域也变了。

修复：

```text
Do not render readable text in the generated video.
The notice board contains aged ink texture only; final readable Chinese characters will be added in post-production.
Keep all non-target regions unchanged: character face, costume, camera angle, background layout, and color grade.
Only edit [specific artifact/prop/region].
```

操作规则：

- 可读文字后期加。
- 编辑任务必须明确 target region 和 non-edit regions。
- 如果只是去字/去穿帮/去水印，先考虑后期修图/视频修复，不要整段重生。

### 22. 共享视觉主题缺失，拼起来像不同项目

来源：aicontentskills/ai-video-storyboard-skill 的 Visual Theme 层。

症状：每个分镜单看还行，剪在一起色温、镜头、颗粒、服装质感完全不同。

修复：每一条 shot prompt 前继承同一个 Visual Theme。

```text
Shared Visual Theme:
Dong Han historical television drama realism.
Palette: #111614 deep green-black shadows, #2E3A35 dark fog, #5C6B63 gray-green midtones, #94A39A diffused haze, #E2EBE0 soft warm highlights.
Lighting: low-key, lifted black point, soft highlight roll-off, volumetric mist, matte skin, no oily shine.
Lens: restrained 35mm/50mm period-drama cinematography, shallow but natural depth of field.
Motion language: locked-off, slow push-in, simple tracking only.

Every shot must preserve this theme exactly.
```

### 23. Prompt extension 把剧情改跑

来源：HunyuanVideo PromptRewrite、Wan2.2 prompt extension、LTX prompt enhancement 的共同风险。

症状：扩写后画面更华丽，但角色关系、剧情事实、道具状态、镜头顺序被改掉；历史剧变成泛古风大片。

修复：把可扩写字段和不可改字段分开。

```text
Locked fields, do not rewrite:
character identity, episode continuity, dialogue/audio ledger, scene location, prop state, shot order, action endpoint, historical period.

Expandable fields:
fabric texture, dust, smoke, practical light, camera framing, natural skin detail, subtle environmental motion.

Rewrite instruction:
Enhance only visual concreteness. Do not add new events, new characters, new props, new camera angles, readable text, or fantasy elements.
```

中文版本：

```text
只允许扩写材质、光线、空气、镜头构图和细节，不允许改人物身份、剧情事实、对白、道具状态、镜头顺序和东汉历史年代。
扩写后必须逐项对照 audio ledger / continuity state / reference usage。
```

### 24. 纯文字无法控制复杂空间路线

症状：人物走位穿帮、镜头穿墙、空间方向错乱、多人站位跳变。

根因：复杂 blocking 和镜头路线超过纯文本视频模型稳定控制范围。

修复路线：

```text
Do not solve complex blocking by adding more adjectives.
Create a blocking reference first: Blender path / storyboard sheet / start-end keyframes / pose-depth control.
Video prompt follows the approved path only.

Camera path:
start at [A], pass [B], end at [C].
Keep floor contact, no wall penetration, no flying camera, no sudden rotation.
Character endpoint: [exact final body position and gaze].
```

适用判断：

- 单人站立、转头、呼吸、慢推：纯 prompt 可处理。
- 两人对白、简单走到门口：prompt + 首帧/尾帧可处理。
- 多人穿插、绕桌、战斗接触、复杂跟拍：先做 Blender/pose/depth/keyframe 控制。

### 25. 多事件时间语义缠绕

来源：Prompt Relay 的 temporal entanglement 思路，Replicate / Seedance time-coded prompt 样本也能互相印证。

症状：后半段事件提前出现；角色提前完成下一段动作；一个时间段里出现了后面才该出现的人/道具/情绪；镜头顺序对，但画面语义串台。

修复：不要写一段大散文让模型自己分配时间。拆成 `Global prompt + Local timeline`，每个 3-5 秒只放一个主要事件。

```text
Global prompt:
Eastern Han historical TV drama realism; same Liu Bei, Zhang Fei, Guan Yu references;
same tavern layout, low-key desaturated green-gray palette, lifted black point,
soft highlight roll-off, volumetric haze, matte natural skin, no subtitles, no readable text.

Local timeline:
00-05s: Liu Bei stands outside the tavern doorway, reads the imperial notice, his right hand slowly tightens on the wooden post.
05-10s: Zhang Fei steps into the doorway from screen right, stops beside Liu Bei, looks at the notice without speaking.
10-15s: Liu Bei turns only his eyes toward Zhang Fei; Zhang Fei lowers his chin once. Both remain silent as the room tone settles.

Do not mention the oath, battle, yellow turban army, or future brotherhood in this clip.
Do not let later events appear early.
```

规则：

- `Global prompt` 只写持续不变的世界、角色、风格、锁定事实。
- `Local timeline` 每段只写一个事件、一个动作端点、一个声音层。
- 不要在本 clip 里提前提未来剧情；未来剧情会污染当前画面。
- 如果 15s 里需要超过 3 个主要事件，拆 clip。

### 26. 高星 Seedance 样本里的“过猛质量词”污染真实历史剧

来源：ZeroLu/awesome-seedance、Replicate Blog 等高热样本常见写法。

症状：prompt 借鉴了高热案例后，画面变成灾难大片、广告片、游戏 CG 或 HDR 展示片；历史剧的布料、皮肤、木器、泥土都过亮过锐。

修复：保留时间码、动作因果、声音细节，替换过猛质量词。

```text
Replace:
8k, ultra realistic, hyper-realistic, cinematic masterpiece, epic scale, HDR, glossy, perfect skin

With:
photorealistic live-action period drama, 35mm/50mm restrained cinematography,
soft diffused side light, lifted black point, soft highlight roll-off,
subtle 35mm grain, real fabric weave, dust in the air, matte skin with natural pores.
```

判断：

- `8K/ultra sharp` 不能修清晰度，常常会制造过锐和 AI 味。
- “director style” 可以用于内部参考，但输出 prompt 应转成具体镜头、灯光、材质、色板，不要依赖名导名词。
- 高热样本里的大场面/高速动作适合测模型上限，不适合直接作为东汉历史剧基调。

### 27. 失败后瞎改 prompt，无法归因

来源：Emily retake protocol、Hao0321/ai-media-generator quality-control 的共同思路。

症状：每次失败后同时改风格、角色、镜头、negative、参考图；结果有时更好有时更坏，但不知道是哪一个变量有效。

修复：先分类，再只改一个变量。

```text
Failure category:
1) Subject integrity: face/product/prop shape changes.
2) Temporal stability: flicker, texture boiling, jitter, horizon drift.
3) Physics realism: cloth, dust, liquid, impact, weight feels fake.
4) Camera control: drifting/orbiting/flying/wall penetration.
5) Style realism: plastic skin, oily face, CGI, over-sharpening.

Retake rule:
Change one variable only:
- prompt clause, OR
- reference image, OR
- camera movement, OR
- negative list, OR
- platform/control workflow.
Keep all other variables unchanged.
Record verdict after each take.
```

历史剧优先级：

```text
先修人物身份/皮肤/服装，再修镜头，再修物理，再修风格微调。
如果主体身份错，不要先调 LUT 或颗粒。
如果运镜穿墙，不要加更多情绪词，升级到 blocking/control。
```

### 28. 物理动作缺少因果链

来源：PhyPrompt、T2VPhysBench、VideoScience-Bench。

症状：布料像漂浮、脚步没有重量、酒水像果冻、打斗没有接触力、尘土乱飘。

修复：每个动作写成“起点 -> 接触/力 -> 介质反应 -> 终点”，而不是只写结果。

```text
Bad:
Zhang Fei angrily slams the table.

Better:
Zhang Fei's right palm drops onto the rough wooden table; the impact pushes the ceramic bowl a few centimeters forward,
wine ripples against the rim, dust lifts from the table edge, then his hand stays pressed flat for a silent beat.
```

历史剧常用物理字段：

```text
foot pressure on dry earth, fabric inertia, sleeve drag, ceramic weight,
wine surface ripple, candle flame response, paper edge flutter, dust settling,
wooden table vibration, bronze weapon weight, breath visible in cold haze.
```

### 29. 生成结果缺了关键概念，却只是在风格上越修越花

来源：VisualPrompter 的视觉反馈/atomic semantic revision 思路。

症状：首帧很好看，但少了关键道具；场景漂亮但人物位置错；修复后风格更复杂，核心错误还在。

修复：先标注缺失/错误字段，再做原子级 prompt 修改。

```text
Observed failure:
The tavern doorway is present, Liu Bei is correct, but the imperial notice board is missing.

Atomic revision:
Add one visible aged wooden notice board on the left foreground, paper surface as unreadable ink texture.
Do not change Liu Bei, costume, camera angle, palette, or lighting.
```

规则：

- 一次只修一个缺失概念或错误关系。
- 保持所有已正确字段不变。
- 不用“make it more cinematic”这类泛化修复。

### 30. 主体运动和相机运动混写

来源：Direct-a-Video、MotionCtrl、Motion Prompting、Wan-Move、I2VControl-Camera。

症状：prompt 同时写“刘备向前走、镜头推近、环绕、跟拍、俯拍转平视”，模型会随机执行，常见结果是眩晕、突然 zoom、人物滑行、空间穿帮。

修复：拆成两个轨道，并给一个可剪辑终点。

```text
subject_motion:
Liu Bei walks from the left side of the table to the doorway, then stops and looks back.

camera_motion:
Slow side tracking shot at chest height, one continuous movement only.
No orbit, no sudden zoom, no top-down transition.

endpoint:
Final 1.5 seconds: Liu Bei stands on the right third of the doorway frame,
side profile visible, looking back into the room.

control_asset:
camera_path_topdown.png + first_frame + last_frame.
```

规则：

- `subject_motion` 只写人物/物体怎么动。
- `camera_motion` 只写一个主运镜。
- `endpoint` 写最后停住的构图。
- 如果必须绕行空间，升级到 storyboard/Blender/trajectory，不再加形容词。

## 31. FPV / 一镜到底路径缺失

来源：`zhouwei713/fpv-immersive-video-prompting`、UCPE、I2VControl-Camera、Blender blocking 经验。

症状：提示词写“宫殿庭院一镜到底经过 5 个角色”，但没有路径、角色顺序、起点/终点、障碍物约束。结果常见：

- 镜头瞬移或中途跳切。
- 角色数量变化。
- 镜头穿过桌椅、柱子、墙、人群。
- 说是低机位/人眼视角，结果变成无人机视角。
- 参考图里的编号/红线残留在最终视频里。

修复：把视频 prompt 前置成“路径资产包”，再写视频 prompt。

```text
asset_pack:
1. numbered_start_frame: scene first frame with small stop numbers 1-5.
2. character_refs: one clean reference image per character.
3. clean_start_frame: same composition without numbers, arrows, text, red lines.
4. optional_topdown_route: camera route map for Blender/blocking only.

route_contract:
camera_identity: human eye-level observer / kneeling servant POV / low cart POV.
start: courtyard gate, facing inward.
stops: 1 Liu Bei under peach tree -> 2 Guan Yu by wine table -> 3 Zhang Fei at entrance -> 4 villagers behind -> 5 altar.
physical_limits: no flying, no wall crossing, no passing through tables, no sudden height change.
endpoint: final 1.5s holds on the altar with all three brothers in the same frame.

negative:
No visible numbers, red line, arrows, text labels, UI markers in the final video.
No added main characters. No teleporting. No drone flight.
```

规则：

- 室内/人物互动优先用编号停靠点，不默认画红线。
- 大世界飞行/地图路径才用红线，且必须给“红线只是控制资产，最终画面不得出现”。
- 首帧管空间，人物参考管身份，干净首帧管最终输入；不要把所有职责塞进一张图。
- FPV 场景 prompt 要回答“摄像机是谁”，否则模型会随意切成航拍、跟拍或第三人称。

## 32. 多镜头转场硬 / clip 之间像合集

症状：

- 每个 clip 单独看还可以，剪到一起像素材合集。
- 场景、人物、道具状态突然重置；上一镜动作没有进入下一镜。
- 镜头之间没有声音、动作、光线或物体承接。

修复：每条 clip 前写状态承接；如果有相邻首尾帧，使用 sliding-window 转场生成。

```yaml
previous_end_state:
  characters: "张飞右手刚拍下木案，身体前倾，怒气未消"
  props: "酒碗震动，案上灰尘被拍起"
  scene: "酒肆外灰绿色晨雾，低饱和环境光"

current_start_state:
  characters: "刘备抬眼看向张飞，手仍扶着草席"
  props: "草席卷边，酒碗仍在前景轻晃"

transition_anchor:
  type: "sound + object motion"
  detail: "拍案声延续 0.8s，酒碗晃动成为下一镜前景"
```

```yaml
transition_clip:
  from_shot: E01-S12
  to_shot: E01-S13
  start_frame: E01-S12_end.png
  end_frame: E01-S13_first.png
  transition_prompt: "Hold Zhang Fei's raised hand for half a second, then pass through the foreground wine bowl into Liu Bei's still face; keep the same gray-green haze and market ambience."
  duration: 4-6s
  closing_hold: 1s stable frame for edit
```

规则：

- 每条 clip 必须有 `previous_end_state` 和 `current_start_state`，除非是明确大场景跳切。
- transition anchor 优先用 action、gaze、sound、object、light-color、occlusion，而不是泛写 smooth transition。
- 有 storyboard/keyframes 时，逐对分析 Shot A -> Shot B，不让模型自己猜中间关系。

来源：EntityBench、VGoT、LoCoT2V-Bench、FramePack、Prompt Relay、StoryGen-Atelier。

## 33. 角色身份锁被长剧情 prompt 稀释

症状：

- 单独生成人物主视图还像，但放进复杂场景或分镜首帧后脸变了。
- 四视图正面相似，侧面/背面发型、颧骨、胡须、体态开始漂移。
- 长 prompt 越写越完整，反而人物不像、服装结构乱、表情僵。

根因：

- 身份 reference、叙事动作、场景信息、镜头语言、风格词、平台参数混在一段里。
- 模型在 identity fidelity、editability、text adherence 之间取舍，复杂叙事会压低身份保持。
- 多人同框时没有 region/role 分离，角色特征互相污染。

修复规则：

```yaml
identity_lock:
  reference: Image1
  locked: face shape, age, beard, hairline, body build, costume silhouette
  allowed_variation: expression, head angle, hand pose, cloth wrinkles

narrative_action:
  one_focused_moment: ...
  body_pose: ...
  prop_interaction: ...

visual_style:
  lighting: low-key desaturated green-gray, lifted black, soft highlight roll-off
  skin: matte low-reflection skin, natural pores, no oily/waxy skin
  material: coarse linen, worn leather, slight dust

reference_roles:
  Image1: identity only
  Image2: costume/four-view only
  Image3: scene environment only
  Image4: first-frame composition only
```

反例：

```text
Use all references to create a realistic cinematic historical scene with Liu Bei, Guan Yu and Zhang Fei...
```

正例：

```text
Image1 is Liu Bei identity reference only: preserve face shape, youth, calm eyes, topknot, slim scholar body.
Image2 is scene environment only: do not copy faces or costumes from it.
Generate a 16:9 first frame: Liu Bei stands center-left under low gray-green daylight, matte skin, coarse linen robe, right hand resting on bamboo mat. Preserve Image1 identity; allow only a slight worried expression.
```

来源：StoryDiffusion、InstantID、PuLID、EditIDv2、Character-Adapter、OpenAI GPT Image guide。

症状：单个 5-15s clip 看起来还行，但剪到一起时：

- 上一镜角色在桌边，下一镜突然站在门口，没有过程。
- 光向/雾气/色温跳变，像换了剧组。
- 道具状态不接，比如酒碗上一镜在手里，下一镜回到桌上。
- 转场只写了 `smooth transition`，模型仍硬切。
- 人物情绪每镜重置，上一镜的愤怒/迟疑/恐惧没有延续。

修复：每个 clip 输出承接字段，而不是只写本镜 prompt。

```text
previous_end_state:
Guan Yu stands behind Liu Bei, right hand resting on the wine bowl, head slightly lowered.

current_start_state:
Same tavern corner, same low-key green-gray haze, Guan Yu still behind Liu Bei with right hand on the bowl.

transition_anchor:
The wine bowl remains the visual bridge; keep it in the lower foreground for the first 1.5 seconds.

audio_bridge:
Carry over the low tavern murmur and the fading ceramic bowl touch sound into the first second.

state_delta:
Only Zhang Fei changes: he steps closer from the doorway and raises his voice.
```

规则：

- 每条 clip 必须有 `previous_end_state` 和 `current_start_state`，否则整集会像素材拼贴。
- 转场锚点优先选可见物：酒碗、火盆、门框、榜文、桃枝、衣袖、手势。
- Audio bridge 比“smooth transition”更实用：环境声延续、脚步声延续、碗声/风声/鼓声承接。
- 状态变化只写一个 `state_delta`，其余人物/道具/光线保持。
- 转场硬时不要先 reroll，先检查上一镜尾帧和下一镜首帧是否真的能接。
