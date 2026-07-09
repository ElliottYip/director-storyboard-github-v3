# 视频/图片质量评价维度

状态：进行中。来源主要是 VBench/VBench++/VBench-2.0、RAPO/PhyPrompt、Google Veo best practices、当前三国项目的失败反馈。

## 视频维度

Seedance 2.0 官方技术报告/发布页可补充为六个“平台贴近型”验收维度：多模态参考生成、复杂音视频指令遵循、复杂运动稳定性、专业镜头语言、音视频表现力、视听一体化协同。它们不是替代 VBench，而是更贴近短剧生产的 checklist。

| 维度 | 看什么 | 常见坏结果 | Prompt / Workflow 修复 |
|---|---|---|---|
| Subject consistency | 同一角色脸、体型、服装是否稳定 | 换脸、年龄漂移、服装变形 | 人物资产前置；每个 clip 复用完整角色描述；换装/受伤/披甲重新出角色表 |
| Background consistency | 场景结构、时代、道具是否稳定 | 宫殿变民居、酒肆布局乱跳 | 场景资产图 + 场景视觉圣经；复杂运镜用场景/路线预演 |
| Motion smoothness | 动作是否连续、自然、有终点 | 卡顿、循环、突然加速 | 每动作写起点/接触点/过程/endpoint；短 clip 单一 focused moment |
| Temporal flickering | 光影/脸/材质是否闪烁 | 皮肤闪、灯光跳、衣服纹理乱 | 减少复杂光源；保持色板和光源方向；避免多风格混写 |
| Human action | 肢体动作、手部接触是否可信 | 手指错、拿碗穿模、打斗乱 | 接触动作写清“手指/物体/力/终点”；复杂动作拆镜 |
| Spatial relationship | 人物/物体相对位置是否清楚 | 对手戏站位变、人物穿桌 | 首帧锁站位；exactly N main characters；前中后景分层 |
| Physics realism | 尘土、火光、布料、酒水是否符合物理 | 尘土乱飘、酒水像胶、衣摆穿模 | 加物理因果：冲击、重力、风向、介质、落回/沉降 |
| Commonsense / Immutability | 不该变化的物体/身份/状态是否保持不变 | 酒碗变形、榜文位置乱跳、衣服忽然变新 | immutable state list；道具状态表；复杂物理拆镜 |
| Imaging quality | 清晰度、噪声、过锐、糊 | 糊、过锐、AI HDR、油光 | 高清首帧；35mm 电影感但不过锐；soft roll-off；matte skin |
| Photoreal artifact control | 是否有生成图常见“假真实”痕迹 | 油亮脸、塑料皮、磨皮、过度锐化、HDR 边缘、物理纹理缺失 | RealGen 式拆分：artifact / realism / details / aesthetics；写自然毛孔、低反射皮肤、真实布料纤维、柔和高光、轻微镜头噪声 |
| Aesthetic quality | 画面是否有统一审美 | 一镜一个风格、像不同剧组 | Visual bible 固定：低调灰绿、体积雾、抬升黑位、轻胶片颗粒 |
| Style alignment | 多张角色/场景/首帧是否像同一部剧 | 同一场戏一张像广告、一张像游戏、一张像古装写真；色温/黑位/高光/皮肤反射不一致 | StyleAligned 式思路：style reference / Visual Bible 独立于剧情；每张图继承色板、光线、镜头响应、材质、颗粒、负面风格词 |
| Prompt adherence | 是否按要求做 | 漏动作、漏人、加字幕 | prompt 分字段；短 clip 少事件；重要约束前置 |
| Semantic preservation | 扩写/修复后是否保留原剧情事实 | 历史剧变玄幻、人物关系/道具/场景被改 | Locked fields + expandable fields；扩写后对照 continuity state |
| Temporal alignment | 事件是否出现在正确时间段 | 后面剧情提前出现、动作顺序乱、时间段互相污染 | Global prompt + local timeline；每 3-5 秒一个事件；不提前提未来剧情 |
| Audio alignment | 声音是否和动作/场景同步 | 泛音效、对白乱、口型差 | Audio 独立句；具体环境声/物体声；对白不用引号 |
| Editability | 镜头是否便于剪辑 | 结尾还在大动作、硬切突兀 | 每镜结尾写可剪辑静态点；环境声淡出；道具/运动方向桥接 |
| Control sufficiency | 这条 prompt 是否需要额外控制素材 | 镜头穿墙、人物绕桌失败、肢体接触乱 | 复杂镜头升级到首尾帧、storyboard sheet、Blender blocking、pose/depth/canny |
| Motion separation | 主体运动和相机运动是否分离 | 人物滑行、突然 zoom、随机环绕 | `subject_motion`、`camera_motion`、`endpoint` 分栏；一个 clip 只允许一个主运镜 |
| Route feasibility | 路径/FPV/一镜到底是否物理可达 | 瞬移、穿墙、穿桌椅、猫视角变航拍、红线编号残留 | camera_identity + stops + physical_limits；编号/红线路径图与干净首帧分离 |
| Reference role clarity | 每张参考图职责是否明确 | 场景图污染人物脸、四视图被当成多人 | @tag/Image1 声明 identity/environment/style/first-frame/storyboard，且与资产表一致 |
| Region / character separation | 多角色和场景参考是否被清晰分区 | 关羽/张飞串脸、场景布料影响人物服装、群像身份混合 | 每角色独立 reference + role/region/position；多人首帧写 left/center/right 和 identity anchors |
| Four-view consistency | 主视图到 front/left/right/back 是否仍是同一人 | 正面像，侧面/背面脸型漂移；发髻/胡须/腰带/鞋履左右不一致 | InstantID/PuLID 式 identity reference + 姿态/关键点/服装结构分离；单人单任务，四视图统一版式 |
| Narrative flow | 多段视频剪在一起是否像同一集戏 | 单 clip 好看但整集像合集，人物动机断裂 | Episode state + observed end state + transition anchor + audio bridge |
| Character development | 角色情绪和关系是否随剧情推进 | 每段表演都像重置，刘备/张飞关系没有变化 | 角色状态表记录情绪、信任、距离、姿态变化；每 clip 只推进一小格 |

## Seedance 官方维度映射

| 官方维度 | 研究解释 | 三国项目验收问题 |
|---|---|---|
| 多模态参考生成 | 是否正确使用人物、场景、动作、声音参考 | 刘备/关羽/张飞是否分别继承对应参考，而不是互相污染 |
| 复杂音视频指令遵循 | 长 prompt 中动作、声音、情绪、shot 是否都执行 | 15s 内是否只做本段指定事件，没有抢跑下一段 |
| 复杂运动稳定性 | 大动作、群体运动、物理交互是否稳定 | 拿酒碗、贴榜文、起身、奔跑、兵器接触是否有力和终点 |
| 专业镜头语言 | 景别、机位、运镜是否像导演设计 | 镜头是否服务戏剧意图，而不是无意义旋转/漂移 |
| 音视频表现力 | BGM、环境声、材质声、对白是否有层次 | 酒肆人声、衣料摩擦、脚步、呼吸是否比“史诗音乐”更具体 |
| 视听一体化协同 | 声音是否跟画面动作严格同步 | 拍桌、拔剑、脚步、雷声、低语口型是否同步 |

官方承认仍需重点验收的风险：

```text
细节稳定性 / 拟真度 / 动态生动性 / 多人口型匹配 / 音频失真 /
多主体一致性 / 文字还原精度 / 复杂编辑误改非目标区域
```

## 图片维度

| 维度 | 看什么 | 常见坏结果 | Prompt / Workflow 修复 |
|---|---|---|---|
| Identity anchor | 人物是否像参考图 | 四视图不像、脸重塑 | 主视图/大头照作为 identity anchor；每张只改角度或服装一类变量 |
| Matte realism | 皮肤是否真实低反射 | 油光、蜡像、磨皮 | natural pores, no beauty filter, matte low-reflection skin |
| Composition clarity | 主体和空间层次是否清楚 | 画面挤、合集、拼图 | 单张图单用途；不要 contact sheet；前中后景分层 |
| Historical accuracy | 是否有现代物 | 现代鞋、拉链、塑料、玻璃钢 | negative modern objects；Details 写汉代材料 |
| Text handling | 文字是否可控 | 乱码、随机字幕 | 不让模型生成关键可读字；后期叠字；背景为不可读墨迹 |
| Lighting continuity | 图与图是否同一剧组 | 色温跳、棚拍感 | 固定直方图/HEX 色板/光源方向；场景图和首帧共享 visual bible |
| Reference usefulness | 图片是否能喂给视频模型 | 太糊、太小、角度混乱 | 人物主视图、半身/大头、场景干净首帧分开导出 |

## 最小人工验收表

每条视频生成后按 0/1 过一遍：

```text
[ ] 角色脸/发型/服装稳定
[ ] 场景和上一镜同属一个视觉世界
[ ] 只出现指定主角数量，无分身
[ ] 镜头只有一个主运镜
[ ] 主体运动和相机运动分开写，且有 ending state
[ ] 路径镜头有明确 camera_identity、stops、physical_limits，且无路径标记残留
[ ] 动作有开始、接触/变化、终点
[ ] 手部/道具接触可信
[ ] 皮肤无油光、无蜡像感
[ ] 真实感不是靠过锐/HDR/磨皮，能看到自然毛孔、布料纤维、轻微镜头纹理
[ ] 无字幕、无水印、无乱码文字
[ ] 镜头结尾可剪辑
[ ] 音效具体且与动作同步
[ ] 事件没有提前抢跑或串到其他时间段
[ ] 当前镜头复杂度和控制素材匹配
[ ] 参考图职责清楚，没有把人物/场景/风格混作一张泛参考
[ ] 不该变化的人物/道具/衣服/场景状态没有变化
[ ] 本段推动了剧情或人物关系，而不是只展示漂亮画面
```
