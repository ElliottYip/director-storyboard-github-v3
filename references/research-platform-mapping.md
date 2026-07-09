# 平台 Prompt 差异映射

状态：进行中。当前基于 GitHub 方法论、官方文档、论文/社区初筛；仍需继续补官方 Runway/Pika/Luma/Kling 来源。Sora 已复核为弃用/历史产品形态来源，不作为当前执行平台。

## 平台快速判断

| 平台/模型 | 当前适合场景 | Prompt 重点 | 风险 |
|---|---|---|---|
| Seedance 2.x | 4-15 秒剧情镜头、多模态参考、短剧片段 | 角色锚点、时间轴、单一主运镜、声音、稳定约束 | 多角色/续写/风格漂移；过多镜头会乱切 |
| Kling | 中文剧情、图生视频、物理交互、音画同步 | Scene -> Characters -> Action -> Camera -> Audio & Style；动作 endpoint | 环境元素过载；图生视频重描述视觉会混乱 |
| Veo | 官方字段清楚，适合做通用视频 schema 参考 | Subject、Action、Scene、Camera、Lens、Lighting、Temporal、Audio | 复杂场景/文字/语音仍需拆短和后期 |
| Runway Gen 系列 | 参考图驱动、单 clip 内一致性、商业镜头 | image reference + concise motion + camera | 跨 clip 连续性不能只靠同一 prompt |
| Pika | 短促效果、Pikaffects、单一主体 | 单主体、少变量、强负面 no morphing | 长剧情与多人连续性弱 |
| Luma Ray 2 | API 化视频生成、start/end keyframes、camera concepts | prompt + keyframes + aspect_ratio + loop + concepts | 官方文档偏 API 参数，不提供完整导演 prompt 方法论 |
| Hailuo/Minimax | 物理动作、短叙事 | 1-2 subjects + temporal action + physical details | 太多主体或抽象风格词会弱化稳定性 |
| Hunyuan/Wan | 开源/本地工作流、I2V/音频字段 | I2V 只描述运动；Wan 可拆 Entity/Scene/Motion/Sound | 需要本地 workflow 和参数，不是纯 prompt 能解决 |
| GPT Image 2 / Nano Banana Pro | 人物四视图、场景图、首帧、storyboard still | 7 层图片 schema；角色/场景/灯光/相机分栏 | prompt 库样本多但方法论弱，需抽样筛选 |
| Kie.ai GPT Image 2 | GPT Image 2 执行平台、批量图片资产 | `createTask` + Bearer auth + `model` + `input.prompt/input_urls` + `aspect_ratio` + `resolution`；多图 role-indexed；结果回调或轮询 | 第三方平台字段，不能当 OpenAI 官方能力；参考图最多/分辨率限制需按页面复核；密钥只在本地运行时读取 |

## 通用转换规则

| 如果目标是 | 不要这样写 | 应该这样写 |
|---|---|---|
| 人物对白 | 多镜头、多机位、复杂环绕 | 固定机位或慢推；写眼神、呼吸、停顿、口型 |
| 人物行走 | “cinematic movement” | stable tracking shot；脚步、衣摆、尘土、终点 |
| 打斗/冲突 | 一条 prompt 塞完整战斗 | 拆为起势、接触、反应、收势；每段一个接触动作 |
| 建立场景 | 直接复杂 FPV | 先大远景/固定机位；只在文旅/地理空间用路线镜头 |
| 图生视频 | 重复描述图里已有服装/构图 | 只写动作、表情、环境变化、相机运动 |
| 首帧图片 | 散文式氛围词 | Meta/Subject/Scene/Lighting/Camera/Details/Negative |

## 图片身份锁平台判断

| 目标 | Prompt-only 风险 | 更稳的执行方式 |
|---|---|---|
| 主视图生成四视图 | 正脸像，侧面/背面脸型漂移；服装结构左右不一致 | 单人单任务，主视图作为 identity reference，再分别约束 front/left/right/back；必要时加姿态/关键点或草图 |
| 多角色同框首帧 | 关羽/张飞/刘备互相串脸、服装串色 | 每个角色独立 reference role；同框 prompt 写清画面区域、站位、服装职责；不要把多张角色图当“风格参考”混输入 |
| 表情/动作状态图 | 身份相似度高但表情僵硬、油光、磨皮 | identity lock 与 expression/action 分栏；RealGen 式拆 artifact/realism/details/aesthetics；负面写 no oily skin / no waxy skin / no over-smoothed face |
| 分镜/首帧资产 | 每一张像不同剧组拍的 | Visual Bible 固定色板、镜头、胶片质感；每张图都继承同一 scene/lighting/material bible |

来源：StoryDiffusion、InstantID、PuLID、Character-Adapter、OpenAI GPT Image guide、RealGen。

## 官方来源细化

### Seedance / 即梦

当前已用 ByteDance Seedance 2.0 官方页面、官方发布博客和技术报告复核硬规则；crowscc/seedance-director、cclank 方法论、Emily2040/seedance-2.0 只作为“实操规则/工作流源”，不得覆盖官方事实。

Seedance 1.0 技术报告补充了平台脉络：Seedance 系列原生强调 multi-shot narrative coherence 和 consistent subject representation。这说明 Seedance 适合 15 秒内的多镜头短叙事，但不等于跨多个独立 clip 自动保持剧集连续；跨 clip 仍需要资产库、状态表、尾帧和 reference roles。

硬规则：

- 单次生成时长 4-15 秒；超过 15 秒必须拆段。
- 单段素材最多 9 张图、3 个视频、3 个音频，混合上限 12 个文件。
- 官方技术报告确认当前开放平台参考输入上限是 3 video clips、9 images、3 audio clips；直接音视频生成输出 480p / 720p。
- 官方技术报告给出访问入口：Doubao、Jimeng、Volcano Engine，model id 为 `doubao-seedance-2-0-260128`。
- 横屏项目优先 16:9；2.35:1 宽银幕应作为构图/后期裁切目标，不要直接假定平台支持。
- @引用必须写清用途：`@图片1 作为刘备角色形象参考`、`@图片2 作为桃园场景参考`，不要只写裸引用。
- 多角色同框时，每个角色绑定独立参考图；提示词中要持续使用同一角色名和同一 @编号。
- 中文对白/音效/BGM 可以直接写入 Seedance prompt；短剧对白不应默认降级为旁白。

官方能力边界：

- 支持文本、图片、音频、视频四种模态输入；用于主体控制、动作控制、风格迁移、特效、视频编辑和视频延续。
- 官方页面展示的高信号 prompt 常包含：单镜/多镜说明、明确声音、动作物理、光线转场、shot 分段、宽高比。
- 官方发布页仍明确承认需要继续改进：细节稳定性、拟真度、动态生动性、多人口型匹配、音频失真、多主体一致性、文字还原精度、复杂编辑效果。

因此 skill 不应把 Seedance 当成“万能一次成片”：

```text
关键文字、榜文、牌匾、字幕优先后期或做成不可读纹理。
多人口型与多人对白要拆镜头，避免多人同时正面讲话。
多主体一致性和复杂编辑要依赖独立角色参考、尾帧、状态表，而不是只靠文字复述。
复杂动作必须给接触点、力、惯性、终止状态；不要只写“激烈打斗”。
```

多段衔接策略：

| 情况 | 优先方案 | 原因 |
|---|---|---|
| 同一场景、同一情绪延续 | 视频延长 / chain extension | BGM、语音、角色和画面衔接最自然 |
| 场景大跳转，如宫廷到乡野 | 独立生成 + 下一段首帧/场景参考 | 避免延展把旧场景拖进新镜头 |
| 中间某段失败需重做 | 独立生成该段或从上一个满意段重新延长 | 视频延长有链式依赖，后段会受前段影响 |
| 蒙太奇/风格切换 | 完全独立分段 | 不追求连续空间，只追求节奏 |

对当前三国项目的迁移：

```text
每 15s 段只承载一个小戏剧动作：张榜震动、刘备凝视、张飞出场、关羽停步、桃园盟誓等。
同一段内最多 3-4 个 shot，尽量避免“远景->奔跑->对白->打斗->转场”全塞一段。
每段 prompt 固定六栏：References / Background / Shot Timeline / Sound / Style / Negatives。
```

### Veo

Google 官方 prompt guide 将视频 prompt 拆成：Subject、Action、Scene/Context、Camera angles、Camera movements、Lens/Optical effects、Visual style/aesthetics、Lighting、Tone/Mood、Artistic style、Ambiance、Temporal elements、Audio、Cinematic terms。

对当前 skill 的硬迁移：

- `Action` 应覆盖 movement、interaction、emotional expression、subtle action、transformation，不只是大动作。
- `Camera angles` 和 `Camera movements` 必须分栏，不能混写。
- `Visual style` 应拆成 Lighting / Tone / Artistic style / Ambiance。直方图、色板、体积雾、材质纹理应进入 Lighting/Ambiance。
- `Temporal elements` 可用于修复转场生硬：短镜头里写清“逐渐、稍微、缓慢演化”的时间变化。
- `Audio` 用独立句子描述，分 sound effects、ambient noise、dialogue。

Google 官方 best practices 还给出工作流规则：

- 短视频每条 prompt 只聚焦单一场景/单一时刻，不把 A then B then C 塞进一条短 clip。
- 角色和声音一致性要复制完整角色描述到每个新场景；只改动作和场景。
- I2V 的源图质量直接决定后续角色细节、灯光和风格；把源图当作 first frame。
- I2V prompt 只写 motion，不重描述源图已有的 subject、scene、style。
- 对白不使用引号，避免模型把引号内文字渲染到画面。

### Luma Ray 2

Luma 官方文档更偏 API 参数和 camera concept 控制：

- 支持 text-to-video、image-to-video、start frame、end frame、start+end keyframes、aspect_ratio、loop。
- Camera concepts 是显式枚举，可通过 `concepts` 参数传入，如 `static`、`push_in`、`pull_out`、`pan_left`、`pan_right`、`tilt_up`、`tilt_down`、`orbit_left`、`orbit_right`、`pov`、`over_the_shoulder`、`aerial_drone`、`handheld`、`dolly_zoom`、`low_angle`、`high_angle`、`eye_level`。
- Modify Video 有 `adhere`、`flex`、`reimagine` 三类强度：轻修复/风格变化/重构世界。可作为后续“修复失败视频”而非“重写 prompt”路线。

对当前 skill 的迁移：

```text
运镜字段应优先选枚举，不要自由堆词。
复杂镜头需要 start/end keyframes，而不是靠一句“平滑转场”。
失败视频如果动作正确但风格差，可考虑 modify/retexture；如果动作也错，应重生成。
```

### LTX / Wan / Hunyuan 开源与 API 生态

这些来源不是短剧 prompt skill，但能帮助划清“prompt 能解决什么 / 需要控制工作流解决什么”的边界。

论文/项目层面的控制证据：

- TIP-I2V 将真实用户 `text + image prompt` 和多个 I2V 模型输出整理成百万级数据集，说明 I2V prompt 不能简单套 T2V 模板。
- AnyI2V 明确指出 text prompt 难以精确控制 spatial layout，转而用 conditional image + trajectories 做 motion control。
- Wan-Move 展示单物体/多物体、camera control、motion transfer、3D rotation 等控制任务，说明复杂运动可以被拆成轨迹资产，而不只是语言描述。
- Direct-a-Video / MotionCtrl / Motion Prompting 这组研究共同支持一个结论：对象运动和相机运动要拆开描述，复杂动态动作需要 trajectory/camera pose/motion parameter，不应把所有动作塞进自然语言。
- MotionStream / CamMimic 进一步把“交互式控制”和“参考视频相机运动迁移”作为方向，说明未来可以把低清预演视频、Blender camera path 或手工路线图当控制素材。
- ComfyGPT / ComfyUI-R1 / ComfySearch 这类研究把 ComfyUI workflow 生成视为独立问题，说明本地节点链路需要规划、校验和执行反馈，不应简化为“一条万能 prompt”。

LTX 官方资料可确认：

- LTX-Video 支持 T2V、I2V、多关键帧 conditioning、keyframe-based animation、video extension、video-to-video。
- LTX-2 / LTX docs 强调同步音视频生成，API 包含 Text-to-Video、Image-to-Video、Audio-to-Video、Retake、Extend。
- I2V 官方描述是：提供 reference image 和 motion prompt，输出应保留 source image 的 visual identity。
- README 的 prompt engineering 建议是：按时间顺序描述 action/scene，包含具体 movement、appearance、camera angle、environment、lighting，单段保持 literal/precise，200 words 内。
- LTX 控制生态包括 depth / pose / canny control LoRA、multiple keyframes、ComfyUI workflows。

Wan2.2 官方资料可确认：

- Wan2.2 官方支持 T2V、I2V、TI2V、S2V、Animate；已集成 ComfyUI/Diffusers。
- ComfyUI 官方 Wan2.2 教程明确覆盖 T2V、I2V、FLF2V；同时有 Fun Control 与 Fun Camera 两条控制教程。
- Fun Control 可用 Canny、Depth、OpenPose、MLSD、trajectory 等控制类型；Fun Camera 可用轨迹/相机控制来减少纯文本运镜失控。
- Wan2.2 强调 cinematic-level aesthetics，并标注 lighting、composition、contrast、color tone 等可控审美标签。
- 官方建议 prompt extension；T2V 可用 Qwen 文本模型，I2V 可用 Qwen-VL 模型扩展提示词。
- Wan-Animate 的 animation/replacement 需要输入视频、角色图，并经过 pose/face/background/mask 等预处理。
- kijai/ComfyUI-WanVideoWrapper 是当前高星本地 Wan 节点生态之一，README 明确它是 WIP/sandbox，且建议 native ComfyUI 已支持时优先 native；因此它适合作为 workflow_hint 来源，不适合直接当“平台参数规范”。
- 其 example_workflows 已可确认覆盖 FLF2V、Fun Control/Fun Camera、ControlNet、WanMove、WanAnimate preprocess、ReCamMaster、MultiTalk/FantasyTalking/Portrait、pose/dancer/stand-in 等方向。

Kijai / WanVideoWrapper 迁移为控制提示：

| 镜头问题 | workflow_hint | 需要输出的控制素材 |
|---|---|---|
| 首帧好看但结尾构图跑偏 | FLF2V / first-last-frame | 首帧、尾帧、动作终点文字 |
| 运镜随机绕、突然推拉 | Fun Camera / ReCamMaster / WanMove | 相机路线、俯视图、start/end camera notes |
| 人物从 A 点走到 B 点不稳定 | WanMove / trajectory / Blender blocking | 人物路径图、空间障碍物、动作端点 |
| 打斗、拉扯、跪拜、多人接触 | WanAnimate preprocess / OpenPose / ControlNet | pose、depth/canny、角色参考、接触点 |
| 对白、口型、说话视角 | MultiTalk / FantasyTalking / talking workflow candidate | 角色图、音频或台词、speaker attribution |

HunyuanVideo 官方资料可确认：

- 支持 T2V 与 HunyuanVideo-I2V，社区/官方生态包括 Avatar、Custom、Keyframe Control LoRA、ComfyUI、Diffusers。
- PromptRewrite 分 Normal 与 Master：Normal 偏理解用户意图，Master 强化 composition、lighting、camera movement，但可能丢失语义细节。

对当前三国项目的迁移：

```text
复杂人物走位、多人调度、稳定运镜路线，不要只靠视频 prompt。
若镜头要求“人物从 A 点绕过桌案走到 B 点，镜头随行并避开柱子”，应优先：
1) Blender / 简易 3D blocking 预演空间和镜头路线；
2) 输出 start/end keyframes 或 storyboard sheet；
3) 能接本地 workflow 时用 pose/depth/canny/keyframe control；
4) 视频 prompt 只描述 approved path、动作终点和风格继承。

Prompt extension 只用于丰富材质/灯光/相机，不允许改剧情事实、角色身份、镜头顺序、道具状态。
Master-style rewrite 有丢语义风险；历史剧应先锁 Audio Ledger / Continuity State / Reference Usage，再允许扩写视觉细节。

运镜字段应拆成：

- `subject_motion`: 人物/马/旗帜/火光怎么动。
- `camera_motion`: 镜头本身怎么动，只允许一个主运动。
- `endpoint`: 最后 1-1.5 秒停在哪个构图和姿态。
- `control_asset`: 无 / 首尾帧 / storyboard sheet / Blender route / pose-depth-canny / reference camera video。

UCPE / CameraCtrl / ReCamMaster / I2VControl-Camera 的共同启发：

- camera movement 不是单纯文字风格词，而是可表示为 camera pose、intrinsics、distortion、orientation、trajectory 的控制问题。
- 如果用户要“跟拍绕柱、低机位推进、再转向门口”，prompt 里应只保留一个主运镜和终点构图，其余交给 Blender route、camera map、reference camera video 或支持 camera control 的 workflow。
- 镜头参数化的目标不是让 `director-storyboard-github` 直接训练模型，而是让它输出明确的控制素材需求：起始机位、终止机位、视线轴、人物路线、障碍物、镜头高度、镜头方向。
```

### Geometry-Guided Short Drama

新增来源：`iLearn-Lab/DramaDirector`、`DrawVideo`、`One Sentence, One Drama`、`SkyScript-100M`。

关键结论：

- 短剧生成的核心问题不是“prompt 不够长”，而是长程一致性、空间一致性、镜头语言结构化、视觉-叙事对齐和生产级 QC。
- `DramaDirector` 把短剧拆成 storyboard planning、text-visual reward、retrieval、first-frame generation、video synthesis，并在预处理阶段产出 shot splitting、keyframes、ASR、structured shot descriptions、depth/pose assets、text/depth/pose embeddings。
- `DrawVideo` 进一步把每个镜头拆成 sketch、appearance prompt、motion prompt：sketch 管 pose/layout/composition，appearance 管 identity/scene/style，motion 管 temporal dynamics。
- `One Sentence, One Drama` 明确提到 3D-grounded first-frame generation、scene transition planning、multi-stage reviewer loops。
- `SkyScript-100M` 说明短剧里 shooting script 是独立数据层，不等于普通剧本摘要。

对当前三国项目的迁移：

```yaml
shot_control_package:
  shooting_script_fields:
    scene: 桃园晨雾
    shot_scale: medium wide
    camera_angle: eye level
    camera_motion: slow lateral track
    duration: 8s
  geometry_reference:
    type: blender_blocking | depth_pose | storyboard_sketch
    assets:
      - E01_S12_blender_topdown.png
      - E01_S12_pose_depth.png
  appearance_prompt:
    locks: [visual_bible, character_refs, scene_ref]
  motion_prompt:
    only_describes: [walking_path, gaze_shift, sleeve_motion, fog_movement]
  reviewer_loop:
    check: [spatial_consistency, visual_narrative_alignment, transition_anchor]
```

这意味着：如果镜头里有人物绕行、三人站位变化、镜头穿过门廊/酒肆/桃林，优先输出 Blender blocking / depth / pose / storyboard sketch 需求，再写视频 prompt。

控制升级路由：

| 镜头复杂度 | 推荐路线 | 不推荐 |
|---|---|---|
| 单人静态表演、转头、呼吸、眼神 | 纯 prompt + 角色参考图 + 场景首帧 | 加过多 camera verbs |
| 单人小步移动、拿起道具、坐下起身 | 首帧 + 尾帧/动作 endpoint；必要时 storyboard sheet | 一条 prompt 同时写多动作 |
| 两人对话、简单走位 | 首帧锁站位 + 反打拆镜 + audio ledger | 同镜头双人同时大段说话 |
| 绕桌、穿门、跟拍、避障 | Blender blocking / 俯视路线图 / start-end keyframes | 只写“cinematic tracking shot” |
| 打斗、拥抱、拉扯、多人接触 | Pose/Depth/Canny/OpenPose/control video；拆成接触前/接触/反应 | 15 秒内完整打斗 |
| 场景穿梭/复杂 FPV | FPV 路线图 + 编号停靠 + 控制视频/轨迹 | 自由发挥式长路线 |

如果后续使用 Blender MCP：

```text
Blender 不需要做成最终画面，它更像导演预演工具：
1) 搭低模场景：门、桌、柱、人物起点/终点；
2) 标相机路径：start -> pass -> end；
3) 导出俯视路线图、首尾构图、depth/canny/pose/trajectory reference；
4) 视频 prompt 只跟随这条 approved path，不再自由发挥。
```

长视频/多事件控制补充：

```text
FramePack 的启发：长视频生成应 progressive，下一段从真实已接受帧/状态继续，而不是从计划文本继续。
Prompt Relay 的启发：多事件 prompt 容易 temporal entanglement，语义会漏到错误时间段。

轻量迁移到 Seedance skill：
global_prompt = visual bible + 角色身份 + 场景状态 + 不可改规则
local_prompts = 每 3-5 秒一个明确事件
segment_lengths = 每段秒数
observed_end_state = 每条生成通过验收后记录真实结尾，下一条从这里开始
```

对 15 秒 prompt 的写法影响：

```text
不要写：刘备看榜、张飞入门、两人争论、关羽出现、众人去桃园。
应该写：
Global: 东汉涿县街口，刘备身份/服装/低调灰绿色 visual bible 不变。
00-05: 刘备停在榜文前，视线从墨迹移到远处。
05-10: 人群声音压低，榜纸边缘被风吹动，刘备手指收紧。
10-15: 刘备转身离开榜文，动作停在可剪辑侧背影。
```

### Kling 官方开放平台

当前已从 `https://app.klingai.com/global/dev/document-api/...` 页面和前端导航 JS 抽到官方能力路径。正文参数仍由 SPA 运行时加载，当前 curl 只能稳定验证目录/入口；进一步读取到的 i18n JSON 主要是界面词和公告词，不能把具体参数当已读官方规则。

可确认的官方文档栏目：

- Video: Kling 3.0 Turbo、Kling 3.0 & 3.0 Omni、Kling O1、Kling 2.6、Kling 2.5 Turbo。
- Video workflow: Text to Video、Image to Video、Omni Video Generation、Motion Control、Element Management、Voice Management。
- Production-adjacent APIs: Avatar、Text to Speech、Text to Audio、Video to Audio、Lip Sync、Face Recognition。
- Historical/hidden lanes in navigation: Kling 2.1 Master、2.1、2.0 Master、1.6、1.5、1.0；其中 1.6/1.5/1.0 仍列出 Video Extension，多图参考、Multi-element Video Editing 等旧路径。
- Image: Kling Image 3.0/Omni、Image O1、AI Multi-Shot、Outpainting、Text/Image to Image、Multi-Image to Image。

对当前 skill 的迁移：

```text
Kling adapter 先只写“能力路径”，不要写未经正文复核的 duration/aspect_ratio/negative_prompt 参数。
如果后续要接 Kling API，应优先复核对应路径：
/api/video/3-0-omni/text-to-video
/api/video/3-0-omni/image-to-video
/api/video/o1/video-omni
/api/video/motion-control
/api/video/lip-sync
```

判断：

- Kling 比 Pika/Runway/Sora 当前更容易确认官方 API 栏目；可以作为“平台能力映射”的官方来源。
- 但 prompt 优化规则仍不能只靠官方导航；需要继续读正文、社区失败案例和实际输出样本。

### Kling 社区 skill / prompt generator

来源：`maciejdzierzek/kling-ai-prompt-generator`。该仓库不是官方文档，但其 SKILL 明确要求关键事实回到 Kling release notes 核验；可作为平台 prompt adapter 的结构参考。

可迁移规则：

- I2V：只描述“什么在动”，不要重述源图里已经存在的主体、服装、构图。
- T2V：按 `Scene -> Characters -> Action -> Camera -> Audio & Style` 编排；复杂动作可用 `First / then / finally`，但每个 shot 仍只保留一个主动作。
- Multi-shot：显式写每个 shot 的秒数、景别、动作、运镜，先写共享 style anchor。
- Motion endpoint：开放动作必须写 `then stops / then settles / returns to starting position`，防止生成结尾悬空或 stuck。
- Element Reference：跨镜人物/物体/场景一致性应走元素绑定或多图参考，不要只靠同一名字。
- Speaker attribution：原生对白要写 `[Speaker: Character Name] "dialogue"` 或对应平台格式；多人对白必须分 speaker。
- 成本策略：先低成本/低分辨率迭代，最后再高质量输出。

对三国项目的 Kling adapter 草案：

```text
Shared style anchor:
Dong Han historical drama, low-key green-gray palette, matte skin, no oily highlights, no modern objects.

Shot 1 (5s): [wide/medium/close], one visible action, one camera movement, one audio layer.
Motion endpoint: the action settles into a cuttable pose in the final 1.5 seconds.
Element references: @liu_bei identity only, @tavern_scene environment only.
Negative: no subtitles, no generated text, no logo, no plastic skin, no duplicate characters.
```

### Sora / Runway / Pika 当前证据状态

- OpenAI Help / Sora 官方页面本轮已可读，但出现两类官方状态文本，需要同时标注日期和适用范围：
  - `openai.com/sora/` 当前重定向到 Sora discontinuation Help Center，页面写明 Sora web/app experiences 已在 2026-04-26 discontinued，Sora API 将在 2026-09-24 discontinued。
  - `Generating videos on Sora` Help Center 页面写明内容只适用于 Sora 1 web，不适用于 Sora app 或 Sora 2 on web；Sora 1 web experience 正在 deprecated，用户应 transition away。
- 因此，后续 skill 不应把 Sora 当成当前主要执行平台；只能把它的 storyboard/recut/remix/blend/loop 概念作为历史/产品形态参考。
- Sora 1 web 生成页仍提供可迁移的工作流线索：可设置 aspect ratio、resolution、duration、variations；Storyboard 允许按 timestamp 添加 image/video/text cards；官方建议 storyboard cards 之间留空，否则更容易 hard cuts。
- Runway Gen-4 Help Center 当前仍返回 Cloudflare challenge，不能收录帮助页参数；但 Runway 官方 API 首页、模型页、Getting Started Markdown 可读。
- Runway 官方模型页列出 `seedance2`、`seedance2_fast`、`seedance2_mini`、`gen4.5`、`act_two`、`veo3/veo3.1`、`gpt_image_2`、`gen4_image` 等模型，能作为 API 执行层来源。
- Runway Getting Started 可确认：视频任务使用 `promptImage`、`promptText`、`ratio`、`duration`；也支持不传 `promptImage` 的 T2V；图片任务支持 `referenceImages`，并用 `@tag` 在 `promptText` 里引用参考图。
- Runway 官方 Gen-4 研究页明确强调：用 visual references 结合 instructions 生成一致的 styles、subjects、locations；单张角色参考图可支持跨 lighting / locations / treatments 的 character consistency；coverage 工作流要求提供 subjects 的参考图并描述 shot composition。
- Pika 官网 API 页明确导向 Fal.ai；Fal 搜索页可读到 `fal-ai/pika/v2.2/text-to-video`、`fal-ai/pika/v2.2/image-to-video`、`fal-ai/pika/v2.2/pikascenes`、`fal-ai/pika/v2/pikaffects` 等 endpoint，但这是第三方执行入口，不是 Pika prompt 官方指南。
- Fal 的 Pika v2.2 I2V 页面可读 request schema：`image_url` 是第一帧，`prompt` 必填，另有 `seed`、`negative_prompt`、`resolution`=`720p/1080p`、`duration`=`5/10`。同站 function list 还显示 `pikascenes` 用多图组合物体，`pikaframes` 用 2-5 个 keyframes 和 per-transition prompts 做关键帧过渡。
- Fal 的 Kling 2.0 Master I2V 页面可读 request schema：`prompt`、`image_url` 必填，`duration`=`5/10`，`negative_prompt` 默认 `blur, distort, and low quality`，`cfg_scale` 默认 0.5、范围 0-1。Fal 页面还列出 Kling V2.6 standard Motion Control：参考视频控制动作、参考图控制外观；orientation=`video` 更适合复杂动作，最长 30s；orientation=`image` 更适合 camera movement，最长 10s。
- Fal 的 Wan FLF2V 页面当前只稳定读到 first-last-frame-to-video 的页面描述：给定 first frame 和 desired end frame，中间桥接平滑运动；完整 request schema 没有稳定读到，因此只作为首尾帧控制路线的旁证。

处理原则：

```text
Runway 可以写官方 API 字段和 referenceImages/@tag 规则；但 Runway Help Center 里的更细 prompt 指南仍未读到。
Pika/Fal 与 Kling/Fal 可以写第三方执行字段，但必须标注 fal.ai schema；暂不写成 Pika/Kling 官方 prompt 细规则。
Fal Wan FLF2V 只能写首尾帧控制思路，暂不写字段名。
Sora 暂不写成可执行 adapter；只保留 storyboard spacing / recut / blend / loop 等产品概念，并标注为 Sora 1 web 历史/弃用状态。
若后续 skill 需要 platform adapter，只能写“待验证字段”和“来自二级资料的推测”，不能把它们标成官方规则。
```

Runway 对当前三国项目的迁移：

```text
每个核心角色至少一张清晰参考图；复杂服装/道具另给图时要标注用途，避免被当成新角色。
图片参考用 @tag 机制显式标注：@LiuBei identity, @Tavern style/location, @MatteSkinLook style。
每个 shot 需要描述 composition：景别、人物左右/前后位置、镜头高度、视线目标。
跨镜头一致性不要只复制同一段文字；要复用 reference image + look/feel + shot composition。
Runway 类平台适合用来验证“参考图一致性/coverage 多角度”思路，不应直接替代 Seedance 的音画短剧 prompt。
```
