# 来源目录与价值判断

状态：进行中。只收可转成规则、模板、诊断项或工作流的来源；不把纯展示型 prompt 堆砌当作核心资料。

## 收录判断标准

| 判断项 | 高权重 | 低权重 / 降权 |
|---|---|---|
| 能否变成规则 | 有明确字段、状态机、workflow、QA、failure taxonomy | 只有漂亮样例、没有方法论 |
| 是否贴近用户问题 | 角色一致、场景统一、运镜/走位、转场、Seedance 15s、KIE 执行 | 泛娱乐、广告转化、纯竖屏 faceless |
| 证据质量 | 官方文档、论文、README 有具体 pipeline、GitHub 高星且代码/文档完整 | 商业导流、空壳 repo、README 只有口号 |
| 可迁移性 | 能落为 `asset_manifest`、`reference_roles`、`shot_ready`、`control_level`、`retake_plan` | 只能复制风格词、平台事实不可验证 |
| 风格风险 | 能帮助真实历史剧降低 AI 感 | 会引入产品广告感、过饱和、8K/HDR/epic 污染 |

结论：星数是热度信号，不是采用理由。低星但能直接解释“运镜怪/转场硬/身份漂移”的项目可以进入核心；高星但只适合广告或 faceless shorts 的项目只抽结构。

## 高价值核心来源

| 来源 | 类型 | 当前价值判断 | 可迁移模块 |
|---|---|---|---|
| YouMind-OpenLab/awesome-seedance-2-prompts | GitHub prompt 库 | 高价值样本库；星数高，样本多，适合抽取成功 prompt 的时间轴/微动作/声音写法 | Seedance 15s 三段式、声音同步、人物稳定约束 |
| cclank/lanshu-awesome-ai-video-kit | GitHub 方法论 + skill | 高价值方法论库；比单纯 prompt 库更适合做 skill | Seedance debugger、Kling 公式、跨模型公式、运镜词典、约束清单 |
| Alisa0808/vibe-creating-skill | GitHub 多平台视频 prompt skill | 中高价值方法论；GitHub API 当前约 92 stars。README 明确 judgment-first：先判断是否适合 vibe creating，再按 visual anchor / action-state / local tonality / video theme 四层补足；强调删除低价值镜头参数，把参数翻译成可见视觉结果。它也明确不适合严格 dialogue sync、step-by-step tutorial、精确 shot-by-shot 执行 | 后续 skill 可加“rewrite mode 判断”：氛围/情绪镜头可删参数并保留故事质感；严格分镜/对白/运镜控制不能被 vibe 改写吞掉。对历史剧尤其适合把 f/ISO/HDR 等词翻译成“柔和高光、抬升黑位、低饱和灰绿雾气、真实布料” |
| neopen/story-shot-agent | GitHub workflow | 高价值架构参考；剧本到分镜到视频 prompt 的链路清楚 | 剧本切片、duration planning、continuity memory、JSON 输出 |
| Vchitect/RAPO | 论文/代码 | 高价值研究参考；适合做轻量 prompt 修复器 | failure taxonomy -> modifier retrieval -> prompt refactor |
| Vchitect/RAPO++ | 论文/代码同源 | 高价值 prompt 优化研究；在 RAPO 基础上加入 cross-stage prompt optimization、test-time iterative feedback、semantic alignment/spatial fidelity/temporal coherence/optical flow 等反馈 | 后续 failure diagnoser 可做“检索修饰词 + 语义保真重写 + 生成后反馈再改”，但实现时要避免过度复杂 |
| POS: Prompts Optimization Suite for T2V | 论文 | 中高价值语义保真证据；指出 LLM 改写 prompt 常导致 semantic deviation，因此需要 semantic-preserving rewriter | Prompt 扩写必须锁剧情事实/角色/道具/镜头顺序；扩写只动视觉细节 |
| IPT2V / TPIGE identity-preserving prompt-image-guidance enhancement | 论文/代码 | 高价值身份一致性研究；训练免费地结合 Face Aware Prompt Enhancement、Prompt Aware Reference Image Enhancement、ID-Aware Spatiotemporal Guidance 来提升参考人物身份保持 | 角色资产生成后应先做 face-aware prompt：把脸型/年龄/发型/体态写入身份锁；参考图和 prompt 冲突时先修参考图，不直接视频生成 |
| Character-Adapter/Character-Adapter | 论文/项目页 | 中高价值角色一致性旁证；约 63 stars，README 强调 prompt-guided region control、training-free、高保真角色定制，可保留 hairstyle、identity、attire 等属性，并支持多角色/风格迁移。代码状态有限，不能当现成执行链 | 对三国角色图：多角色不要把所有参考图混作一张泛参考；应明确每个角色的区域/身份/服装职责，避免场景图污染脸、关羽/张飞互相串脸 |
| HVision-NKU/StoryDiffusion | GitHub/论文，长程角色一致性 | 高价值核心证据；约 6.4k stars，NeurIPS 2024 Spotlight。README 明确用 consistent self-attention 做长程角色一致图片，并建议至少 5-6 个文本 prompts 改善 layout；视频阶段用 condition images 做长视频/转场 | 对“人物四视图 + 多镜头首帧”：先生成一组一致角色/状态图片，再把这些图片作为后续视频 condition/reference；不要期望每个镜头从零 prompt 都自动长得一样 |
| instantX-research/InstantID | GitHub/论文，单图身份保持 | 高价值身份锁证据；约 12k stars。README 明确 zero-shot、single image ID-preserving generation；实现使用 face embedding + face landmarks/keypoints，tips 说明相似度/文本控制/过饱和需要调 adapter/control 权重；当前限制是 multi-person 未支持，只取最大脸 | 对“四视图”：主视图脸要作为 identity reference，但侧面/背面需要姿态/关键点/服装结构另行约束；多人合图不适合混用单脸身份锁，必须单人单任务或区域分区 |
| ToTheBeginning/PuLID | GitHub/论文，ID customization | 高价值身份锁旁证；约 3.5k stars，NeurIPS 2024。README model zoo 反复强调 ID fidelity、editability、facial naturalness、similarity 的权衡，并列出 ComfyUI 实现 | 对人物素材链：身份相似度和画面可编辑性是 tradeoff；如果“脸像了但表情/服装/侧脸不自然”，应分阶段调，不要一条 prompt 同时追求全部 |
| typemovie/EditIDv2 | GitHub/论文，复杂叙事身份编辑 | 中高价值身份锁旁证；约 6 stars 但来自 iFlyTek Typemovie Research。README 明确复杂长文本叙事会导致 editing capability 降低、semantic understanding bias 和 identity consistency breakdown；EditIDv2 目标是在 complex narrative environments 中保持身份一致和语义编辑 | 对历史剧长 prompt：人物身份锁不应塞进整段场景散文；应把 identity prompt、narrative prompt、visual style prompt 分开，先保身份，再局部编辑场景/表演 |
| yejy53/RealGen | GitHub/论文 photorealism | 高价值真实感诊断旁证；约 335 stars，README 提出 detector-guided rewards 和 RealBench，用检测器/arena scoring 评估 photorealism、artifacts、realism、details、aesthetics，并把“texture and fidelity of real physical world”作为优化方向 | 对“AI 感/油光/塑料皮/糊”的迁移：不要只写 photorealistic，要写可检测的真实世界纹理、自然毛孔、低反射皮肤、布料粗糙度、柔和高光 roll-off、轻微镜头噪声；postflight 可把 artifact/realism/detail/aesthetic 分开验收 |
| RISE-T2V | 论文 | 中等价值 prompt rewrite 研究；强调短 prompt 对 T2V 不够，LLM rephrasing 可增强语义，但与 POS 的语义偏移风险互为约束 | 可支持“扩写有用”，但后续 skill 必须加 locked fields，避免扩写改剧情 |
| PhyPrompt | 论文 | 中高价值物理 prompt 优化研究；用物理-focused 数据和奖励课程同时优化语义忠实与物理常识，说明物理约束需要显式加入但不能牺牲语义 | 历史剧动作 prompt 应补接触点、力、惯性、重力、落回；打斗/碰撞仍需拆镜和控制素材 |
| VisualPrompter | 论文/代码 | 中高价值图像 prompt 反馈修复源；通过视觉反馈找缺失概念，再进行 atomic semantic-level prompt revision，避免只让图更美但内容错 | 图片/首帧失败后应先识别缺失/错误字段，再局部修 prompt；适合后续自动 QA 与 retake |
| Vchitect/VBench | 论文/评测工具 | 高价值诊断维度；把“视频不好”拆成可评估项 | subject/background consistency、motion smoothness、flicker、imaging quality |
| VBench-2.0 | 论文/评测框架 | 高价值诊断维度升级；从表层美观/一致性推进到 intrinsic faithfulness，覆盖 Human Fidelity、Controllability、Creativity、Physics、Commonsense | 后续 QA gate 增加人体可信、控制遵循、物理/常识、构图完整性，不只看“画面美” |
| LoCoT2V-Bench | 长视频/复杂 prompt benchmark | 高价值长剧集评估参考；强调 event-level alignment、fine-grained temporal consistency、content clarity、narrative flow、emotional response、character development | 5-10 分钟剧集验收要看事件级对齐、跨事件一致、叙事流，而不是只看单 clip |
| Catherine-R-He/EntityBench | GitHub/论文 benchmark | 高价值长程多镜头一致性证据；约 7 stars 但作者/来源强，README 明确 140 episodes、2,491 shots、per-shot entity schedule，同时跟踪 characters、objects、locations，提出 EntityMem：生成前把 verified per-entity visual references 存入 persistent memory bank | 后续 skill 需要 entity registry：角色、道具、地点都要有跨镜头 schedule 和 verified reference；不能只靠上一条 prompt 记忆。每个镜头前列出 present/absent entities、状态、上次出现间隔 |
| VideoGen-of-Thought (VGoT) | 论文 | 高价值多镜头流程证据；把 multi-shot video 拆成 Script Generation、Keyframe Generation、Shot-Level Video Generation、Smoothing Mechanism；强调 narrative design、character dynamics、background continuity、relationship evolution、camera movement、lighting，以及 adjacent latent transition/smoothing | 对“转场硬”：prompt 层要输出 previous_end_state / next_start_state / transition_anchor / audio_bridge；图像层要用关键帧/首尾帧承接；生成层需要 shot-level clip 和 transition QA |
| Multi-Shot Character Consistency / Video Storyboarding | 论文 | 中高价值角色跨镜头一致性证据；指出多镜头同角色要在身份保持和自然运动之间平衡，简单共享特征可能损伤动态 | 后续 skill 不能为了一致性把人物写死；每镜要分 identity_locked 与 expression/action_allowed，允许表情/姿态变化但锁脸型、发型、服装状态 |
| Gloria: Consistent Character Video Generation via Content Anchors | 论文 | 高价值角色视频一致性证据；2026 论文提出用 compact anchor frames 表达角色视觉属性，针对长时长、多视角、表现性身份一致。还指出 reference-based video generation 会遇到 copy-pasting 和 multi-reference conflicts | 对四视图/状态图：角色不是一张主视图，而是一组 content anchors；四视图、表情、服装状态、手持道具应作为 anchor set。视频 prompt 要声明使用哪些 anchor，避免一股脑塞多图导致复制粘贴或参考冲突 |
| Action2Dialogue | 论文 | 中高价值对白/叙事状态证据；把每场景拆成 setting prompt + character behavior prompt，再结合 representative frame 生成角色台词；用 Recursive Narrative Bank 维持跨场景对白历史与角色目标 | 后续 skill 的 audio ledger 需要角色目标/关系/前文台词状态，不能每个 clip 独立写对白。历史剧对白应跟随场景图/代表帧和角色行为，不只从剧情摘要生成 |
| T2VPhysBench | 物理一致性 benchmark | 中高价值物理失败证据；指出详细物理提示也不能完全修复模型对刚体碰撞、重力、守恒等基本规律的违反 | 复杂物理/碰撞/打斗不能靠 prompt hint 硬救；需要拆镜、控制素材或后期 |
| VideoScience-Bench | 科学/物理推理 benchmark | 辅助物理评估源；提出 Prompt Consistency、Phenomenon Congruency、Correct Dynamism、Immutability、Spatio-Temporal Continuity | 对历史剧可迁移为：现象是否符合预期、动态是否正确、角色/物体状态是否不该变时保持不变 |
| zhouwei713/fpv-immersive-video-prompting | GitHub skill | 高价值专项；低星但非常贴近“运镜/走位怪”问题。README 强调 FPV 不是画面描述，而是行动轨迹设计；用 8 个问题固定 camera identity、起点、目标数量、顺序、物理可达、互动、身份保持、禁忌。对室内人物互动用编号停靠点，对大世界飞行用红线路径控制 | 路径先行、编号停靠、干净首帧、独立人物参考图、物理可行路线、禁止穿墙/飞行/新增角色；可直接迁移到宫殿/庭院/桃园/酒肆一镜到底 |
| kkunkunya/image-prompt-layers | GitHub 小项目 | 星低但结构价值高 | 图片 prompt 7 层 schema |
| rediumvex/ai-video-generator-claude | GitHub skill | 中高价值但需限制用途；GitHub API 当前约 237 stars，README 是 10 个 Seedance/Higgsfield 竖屏营销技能，强调 2 秒 hook、4-15s timeline、camera movement encyclopedia、lighting presets、sound design stack、@image/@video/@audio material reference。商业/奢侈品味道重，不适合照搬到历史正剧 | 可迁移 timing/audio/reference system、首 2 秒注意力钩子、sound-visual sync；不迁移“studio-quality/viral/奢侈品”语气。历史剧里改成“开场冲突/信息钩子 + 声音账本 + 克制运镜” |
| crowscc/seedance-director | GitHub skill | 低星但高相关；与当前 director-storyboard-github 目标接近，适合抽取平台操作规则而不是照搬工作流 | Seedance 15s 分段、视频延展、@引用规范、六段式 prompt、场景策略 |
| AGI-Ruby/ai-GPT_Image2-Seedance_2.0-video-skills | GitHub skill | 低星但结构价值高；把 GPT Image 2 storyboard 与 Seedance prompt 分成两阶段，适合解决首帧/分镜和视频执行脱节 | 角色参考锁 + storyboard keyframe 锁、负面约束、分镜先行 |
| yubowen123/AIYOU_open-ai-video-drama-generator | GitHub 短剧平台 | 中等星但高相关；README 自评产品还不成熟，不能当生产级工具；可抽取短剧节点链路和风格指南 | 创意->剧本->角色->分镜->视频节点链路、REAL/ANIME/3D 风格分类、历史正剧风格 |
| Anil-matcha/Open-AI-Micro-Drama-Generator | GitHub 高星短剧工作台 | 中高价值工作流参考；约 367 stars，README 明确 screenwriter -> character extraction -> storyboard -> portraits -> first frames -> I2V clips -> concat。Prompt 方法论较浅，平台模型事实需复核 | 角色抽取、角色肖像前置、每 shot 首帧、I2V 5s clip、并行生成、最终拼接、SSE 进度可视化 |
| ArcReel/ArcReel | GitHub 高星 AI 视频工作台 | 高价值核心来源；约 3k stars，明确覆盖小说->角色/线索->分集->剧本 JSON->角色设计图->分镜图->视频片段->合成 | 资产前置、角色一致性、线索/道具跨镜追踪、风格参考图、三种视频生成模式、Agent/Subagent 分工 |
| HBAI-Ltd/Toonflow-app | GitHub 高星短剧工作台 | 高价值生产链参考；GitHub API 当前约 10.9k stars。README 明确“策划 -> 编剧 -> 分镜 -> 出片”、无限画布、三层 Agent 协作、持久化记忆、章节事件图谱、ScriptAgent/ProductionAgent、素材/角色/场景/分镜/视频节点。Demo 显示 Seedance 2.0 + GPT Image 2 生产链 | 后续 skill 可借鉴：章节事件图谱、ScriptAgent/ProductionAgent 分离、素材节点化、并行生产、模型供应商适配、成本记录。注意它是应用工作台，不是 prompt 方法论源；抽工作流，不抽默认视觉风格 |
| Forget-C/Jellyfish | GitHub 高星短剧工作台 | 高价值核心 workflow；GitHub API 当前约 4.8k stars。README 把 consistency 作为 first-class problem，维护 characters/actors/scenes/props/costumes，共享 entity model；每个 shot 经过 script breakdown -> shot preparation -> candidate confirmation -> shot ready -> generation workspace；支持 keyframes、reference images、video prompt preview、batch pre-check、async task center | 对后续 skill 最关键：`shot_ready` 状态。只有角色/场景/道具/服装/对白候选确认并链接资产后，才生成首帧和视频；所有图/视频任务必须回写 shot/media system，保留 task id、输入素材、结果和 retake 记录 |
| 0xsline/StoryGen-Atelier | GitHub 高星 storyboard-to-video 工具 | 中高价值转场工作流；GitHub API 当前约 903 stars。README 明确用 Gemini 生成 storyboard text/frames，再用“Interpolation Chain / Sliding Window”逐对分析 Shot A -> Shot B，生成 transition prompt 和 duration，发送 start frame + end frame 到 Veo，最后 FFmpeg concat | 对“转场硬”很直接：相邻分镜不要独立生视频，应做 pairwise transition analysis；每段 clip 明确 start frame、end frame、transition prompt、duration，末尾生成 closing hold |
| iLearn-Lab/DramaDirector | GitHub/论文，短剧几何引导生成 | 低星但极高相关；2026-06 新项目。README 明确短剧生成的问题是 long-horizon consistency、plain text prompts 缺少 camera language/layout/dialogue/duration、visual-narrative mismatch；流程包含 storyboard planning、text-visual reward、retrieval、first-frame generation、video synthesis，并在预处理阶段生成 depth/pose assets 与 embeddings | 对用户“Blender 搭运镜/人物走动”非常关键：短剧镜头不只需要 prompt，还需要 depth/pose/geometry reference。后续 skill 可增加 `geometry_reference` / `blocking_reference` 字段，把 Blender 预演、DWPose、Depth、路线图纳入首帧和视频生成输入 |
| DrawVideo | 论文，storyboard sketch-guided long video | 中高价值控制证据；2026-05 论文指出单一长 prompt 难以控制 pose、composition、layout、motion；把长视频拆成多个 shot，每个 shot 由 sketch、appearance prompt、motion prompt 定义 | 对三国横屏剧集：分镜草图/导演机位图控制构图和姿态，appearance prompt 锁身份/场景/风格，motion prompt 只管时间动态。可作为 Storyboard Lock Generator 的理论补强 |
| One Sentence, One Drama | 论文，短剧多 Agent 生成 | 中高价值流程证据；2026-05 论文把短剧失败归为 narrative pacing、spatial consistency、production-level QC 三类，并提出 3D-grounded first-frame generation、multi-stage reviewer loops、scene-level BGM matching、transition planning | 对后续 skill：不是只输出分镜；还要有空间一致性、BGM/声音桥、分阶段审片与 targeted revision。与 `Shot Ready Gate`、`Transition Builder`、`Retake Log` 直接对齐 |
| vaew/SkyScript-100M | GitHub/论文，短剧 shooting script 数据集 | 高价值数据范式参考；约 1B pairs of scripts and shooting scripts，来源 6,660 部短剧、约 80,000 集、约 10M shooting scripts。不是 prompt 库，但说明“剧本 -> shooting script/镜头语言”是短剧生成的独立数据层 | 后续 skill 应把“剧本改写”和“拍摄脚本/分镜语言”分开；镜头语言字段要结构化：场景、镜头、人物位置、对白、时长，而不是散文式 prompt |
| thoxakihiko/shotlist-forge | GitHub/NPM shotlist 工具 | 低星但结构清晰；把单一 concept 拆成有节奏的 shot-by-shot prompt sequence | continuity flags、beats->shots、duration distribution、pattern 化景别/运镜 |
| Google Cloud Veo prompt guide | 官方文档 | 高价值官方字段参考 | Subject/Action/Scene/Camera/Lens/Lighting/Audio/Temporal |
| Google Cloud Veo best practices | 官方文档 | 高价值官方工作流参考 | 短视频单场景、I2V motion-only、高质量首帧、角色描述复用、避免引号触发文字 |
| Gemini Image best practices | 官方文档 | 高价值图片 prompt 基础规则 | 具体描述、上下文、逐步拆复杂画面、相机控制 |
| OpenAI GPT Image prompting guide | 官方 Cookbook/文档 | 高价值图片资产源；官方说明 gpt-image-2 适合 photorealism、identity-sensitive edits、multi-panel compositions、text rendering、multi-image reference；同时建议多图按索引描述、迭代时小步单变量修改 | GPT Image 2/KIE 图片阶段规则：多图引用必须声明 role，身份敏感资产用 high/medium，复杂图分步，文字后期优先，debug 时一次只改一个变量 |
| Kie.ai GPT Image 2 API page | 第三方执行平台文档 | 高价值执行字段源；页面可读。公开示例确认 `POST https://api.kie.ai/api/v1/jobs/createTask`、Bearer auth、`model:"gpt-image-2-text-to-image"`、`input.prompt`、`aspect_ratio`、`resolution`，结果可用 `callBackUrl` 接收或 task id 轮询。Text-to-Image 字段为 `prompt/aspect_ratio/resolution`，Image-to-Image 另有 `input_urls`；支持 1K/2K/4K，参考图最多 16 个、单文件 30MB；2K/4K 不支持部分极端比例 | 后续 KIE 图片生成脚本应把人物/场景/首帧/分镜分任务提交；多图输入必须 role-indexed；横屏用 16:9，必要时 21:9，但注意分辨率比例限制。注意：KIE 是第三方执行层，不等于 OpenAI 官方 API；本地 RTF 里的密钥不进入研究文档 |
| Luma API docs / Camera Concepts | 官方文档 | 高价值平台参数参考 | start/end keyframes、loop、aspect ratio、camera concept 枚举、modify video mode |
| ZhiHui6/zhihui_nodes_comfyui | GitHub/ComfyUI 中文节点生态 | 辅助来源；约 159 stars。README 覆盖 55+ 节点：文本合并/删除/替换、PromptExpander、PhotographPromptGenerator、WanPromptGenerator、PromptGallery、Qwen3-VL/Florence2 视觉理解、图像尺寸/对比/切换等 | 对 skill 的启发是“prompt 组合要可控”：locked fields、style blocks、negative blocks、平台参数应像节点一样启停/替换/对比；图像分析节点可用于生成后 QA 和首帧 histogram/内容检查 |
| ByteDance Seedance 2.0 official page / launch blog / paper | 官方页面 + 技术报告 | 高价值权威源；已核验 text/image/audio/video 四模态、4-15s、480p/720p、3 视频/9 图/3 音频、Doubao/Jimeng/Volcano Engine/model id。发布页也明确承认细节稳定性、拟真度、多人口型、多主体一致性、文字还原等仍需优化 | Seedance 平台硬规则、官方示例 prompt、官方失败边界、质量评测维度 |
| Seedance 1.0 technical report | 官方/论文 | 辅助平台脉络；强调原生 multi-shot narrative coherence、consistent subject representation、text-to-video + image-to-video 联合学习、video-specific RLHF | 支持 Seedance 适合 15s 多镜头短叙事，但仍需要角色/场景/状态表维持跨 clip 连续 |
| Seedream 2.0 technical report | 官方/论文 | 辅助图片资产源；强调中英双语、中文文化细节、文本渲染、结构正确性、instruction-based editing 与图像一致性 | 如果 KIE/OpenAI 图像不理想，可作为“中文历史文化图像模型”候选；但当前执行平台仍按用户 KIE API |
| Emily2040/seedance-2.0 | GitHub 高星 Seedance skill OS | 高价值核心来源；约 1.6k stars，结构完整，有 README、SKILL.md、28 个子 skill、58 个 reference、eval/scripts。注意：它不是官方文档，平台事实必须逐条复核 | 导演引擎、长剧集状态、续写 source gate、失败 atlas、retake 协议、reference role map、平台事实隔离 |
| beshuaxian/higgsfield-seedance2-jineng | GitHub Seedance/Higgsfield 技能集 | 中高价值镜头词库；约 640 stars，15 个风格 skill，中文/英文齐全。适合作为短视频 hook、镜头/灯光/时间码模板，不适合作为长剧集状态机 | 2 秒 hook、15s 时间线、电影镜头词、灯光/焦点/色彩层级、Higgsfield 素材限制 |
| LichAmnesia/awesome-ad-video-prompts | GitHub 视频广告 prompt 库 | 中等价值；约 157 stars，52 条原创广告视频 prompts，覆盖 Seedance/Veo/Kling/Runway。README 强调 multi-beat timing、real camera language、fidelity guards、duration/aspect/schema。缺点是广告/电商调性强，不适合直接迁移历史剧风格 | 可抽结构：`6s/8s + aspect + 0-2s/2-4s/4-6s + 单一主物理事件 + no deformation/drift/artifacts + implied sound`；不抽产品 hero、品牌露出、过度饱和商业灯光 |
| IgorShadurin/app.yumcut.com | GitHub 短视频生成器 | 辅助工作流；约 712 stars，面向 TikTok/Reels/Shorts 的 self-hosted end-to-end generator：idea -> script -> voice -> visuals -> captions -> final edit，多语言/批量/成本控制。更偏 faceless vertical growth，不是历史剧导演工具 | 借“批量版本测试、脚本/配音/字幕/视觉装配、成本控制、provider choice”；不借竖屏 faceless 内容逻辑 |
| Curated-Awesome-Lists/awesome-ai-talking-heads | GitHub talking-head 资源库 | 辅助来源；高星/老牌列表，聚合 SadTalker、GeneFace、Wav2Lip、IP_LAP、VideoReTalking 等口型/头部运动/身份保持方法。列表本身生成感强，不能当权威综述 | 对短剧对白镜头的启发：口型、头动、表情和身份保持是独立问题；多人对白/强运镜/复杂表情不应全部交给 Seedance，一些场景可拆为半身固定镜头或专业 lipsync 后处理 |
| KlingAI Open Platform | 官方平台文档入口 | 高价值官方入口；SPA 可验证文档路径和能力地图；i18n JSON 只含界面/公告词，具体接口正文仍需登录态或浏览器复核 | Kling 3.0 Turbo、3.0/Omni、O1、2.6、2.5 Turbo、Motion Control、Element/Voice Management、Avatar、Audio Generation、Lip Sync、Video Extension；暂不写未经正文复核的参数 |
| maciejdzierzek/kling-ai-prompt-generator | GitHub Kling skill | 中高价值；README/SKILL 有清晰的 I2V/T2V/Multi-shot/Avatar/Motion Control 模板和常见问题表。模型/价格/发布日期需回到 Kling release notes 核验 | I2V 只写运动、motion endpoint、Element Reference、speaker attribution、1080p 迭代后 4K、Kling 多模式适配 |
| aicontentskills/ai-video-storyboard-skill | GitHub 低星 skill | 星低但结构有用；核心是“共享视觉主题层 + 多 shot prompt + 后期清单”，能解决多段视频像不同人做的 | Visual Theme、shot cadence、prompt 40-80 words、后期 LUT/transition/BGM checklist |
| cliprise/awesome-image-to-video-prompts | GitHub I2V prompt/workflow 库 | 星低且商业导流明显，降权；但 motion-first 原则、源图 QA、I2V negative prompt 和 final beat 模板实用 | 图生视频 motion-only、preserve/move/final beat/restrictions、源图质量检查、禁止文字/Logo/脸手变形 |
| Arch-Dog/video-prompt-engineer | GitHub Seedance 短剧 skill | 低星但高度贴合；已读 README、SKILL、prompt-formula、shot-composition、audio-handling、narrative-transcoding、quality-rubric。价值在“短剧生产规则”，不是平台官方事实 | 音频账本、10-15 秒一致性容器、同空间拆镜衔接锚、情绪外化、强意图运镜、每条 prompt 独立可执行 |
| PUSHINGSQUARES/pushing-creation | GitHub 视觉方法论/skill | 星低但方法论成熟；对图片首帧/风格统一尤其有用。它把 AI 当 DP 来 brief，并用 STYLE/NEG blocks 对抗模型默认坏习惯 | STYLE_PHOTOREAL_BASE、STYLE_REAL_SKIN、STYLE_GRAIN_HALATION、NEG_AI_PLASTIC_SKIN、NEG_AI_LIGHTING、camera/lens/lighting state 字段 |
| JOZUJIOJIO/cinematic-prompts | GitHub Seedance skill | 星低但结构与 Seedance 槽位很贴合；平台约束里“1080p-2K”等仍需官方复核，不能当硬事实 | @槽位注册、六层提示词、时长判定、素材准备清单、操作指引；需删除“8K超高清”类过猛质量词 |
| Lightricks/LTX-Video + LTX docs | 官方 GitHub/API docs | 高价值开源/平台证据；官方 README/docs 明确 T2V/I2V/audio-to-video、multi-keyframe、video extension、retake、depth/pose/canny control、ComfyUI workflow | 复杂动作走 keyframe/control/retake/extend；prompt 200 words 内按 action -> movement -> appearance -> environment -> camera -> lighting 写 |
| Wan-Video/Wan2.2 | 官方 GitHub | 高价值开源视频生态；官方 README 明确 T2V/I2V/TI2V/S2V/Animate、ComfyUI/Diffusers、prompt extension、Wan-Animate preprocess | 本地/ComfyUI 侧可用 prompt extension、speech-to-video、pose/audio/character reference；复杂人物动画不靠纯 prompt |
| Tencent-Hunyuan/HunyuanVideo | 官方 GitHub | 高价值开源模型证据；官方 README 明确 HunyuanVideo-I2V、PromptRewrite、ComfyUI、Diffusers、Avatar/Custom/Keyframe Control 社区生态 | Prompt rewrite 分 normal/master；master 强化构图/灯光/运镜但可能丢语义，后续 skill 可做“扩写但保护剧情字段” |
| lllyasviel/FramePack | 官方 GitHub/论文项目 | 高价值长视频漂移控制证据；next-frame-section 逐段生成，把上下文压缩到常量长度，目标是 drift prevention | 对长剧集提示词的启发：逐段生成、实时验收、用已接受帧作为下一段真实上下文，别从计划文本硬续 |
| GordonChen19/Prompt-Relay | GitHub/论文项目 | 高价值时间控制研究；解释 multi-event prompt 的 temporal entanglement，并用 global_prompt + local_prompts + segment_lengths 路由语义到时间段 | 后续 skill 可做轻量版：global visual/character anchor + timed local prompts，防止事件互相污染 |
| dashtoon/hunyuan-video-keyframe-control-lora | GitHub 控制 LoRA | 中高价值控制证据；用 first/last keyframes 控制开始和结束帧，推荐单主体、人像更稳 | 复杂动作/转场应有首尾帧；不是所有运动都靠一段文字描述 |
| ZeroLu/awesome-seedance | GitHub 高星 Seedance prompt/resource 聚合 | 高星资源入口；GitHub API 当前约 2k stars，README 收集 X/微信/Replicate 等 Seedance 2.0 案例。价值在样本密度和热度，不是导演级 workflow，也不是官方事实源 | 15s time-coded prompt 样本、短剧/广告/电影/转场分类、参考素材组合案例；需要筛掉 8K/hyper-realistic 过度质量词 |
| Replicate Blog: How to make remarkable videos with Seedance 2.0 | 平台实操文章/API 示例 | 高价值非官方实操源；明确多模态引用、参考图/视频/音频、同引擎音频、15s 多 shot 时间码、API 字段。注意它鼓励 `hyper-realistic, 8k`，与当前真实历史剧目标有冲突，需要改写为胶片/镜头/材质语言 | `[Image1]/[Audio1]` 引用、音频长度匹配 duration、参考类型组合、wide->medium->close-up->ECU 15s 升级结构、时间码内写 camera position/subject action/lighting state/transition |
| ComfyUI Wan2.2 官方教程 + Fun Control/Fun Camera | 官方本地工作流文档 | 高价值控制工作流证据；Wan2.2 官方 ComfyUI 页面列出 T2V/I2V/FLF2V、Fun Control 和 Fun Camera。Fun Control 支持 Canny/Depth/OpenPose/MLSD/trajectory，Fun Camera 支持 pan/zoom 类相机控制 | 后续 skill 的“控制升级路由”：复杂走位/运镜 -> 首尾帧、OpenPose/Depth/Canny/trajectory、相机控制码或 Blender blocking；简单镜头才走纯 prompt |
| kijai/ComfyUI-WanVideoWrapper | GitHub 高星 ComfyUI/Wan 节点生态 | 高价值本地 workflow 证据；GitHub API 当前约 6.5k stars。README 明确它是 WIP/sandbox，若 native ComfyUI 已支持某能力应优先 native。example_workflows 覆盖 FLF2V、Fun Control/Fun Camera、ControlNet、WanMove、WanAnimate preprocess、ReCamMaster、MultiTalk/FantasyTalking/Portrait、pose/dancer/stand-in 等 | 迁移为 workflow_hint：精确首尾构图 -> FLF2V；空间走位/相机路线 -> Fun Camera/ReCamMaster/WanMove；人物表演/肢体 -> WanAnimate preprocess/OpenPose/ControlNet；对白/口型 -> MultiTalk/FantasyTalking。它证明复杂运动是工作流问题，不是一段长 prompt 问题 |
| TIP-I2V dataset / ICCV 2025 | 论文 + 数据集 | 高价值研究证据；1.70M+ 用户 text+image prompts 和五个 I2V 模型生成结果，说明 I2V prompt 与 T2I/T2V prompt 不同，应单独建规则 | I2V 阶段必须 motion-first；后续可从真实用户 prompt 数据抽常见失败/偏好，但本轮不直接下载大数据集 |
| VideoGen reference-guided T2V | 论文 | 中高价值理论支撑；使用参考图提升视频视觉 fidelity，并让视频生成更聚焦 dynamics/temporal consistency | 支持当前 pipeline：先生成高质量首帧/分镜/角色资产，再让视频模型执行运动，不从纯文本直接生成复杂剧集 |
| StyleAligned / AlignedGen | 论文，跨图风格一致性 | 高价值风格一致性证据；StyleAligned 指出仅靠同一 style prompt 很难保持 generated image set 风格一致，用 minimal attention sharing 和 reference style inversion 实现 style-consistent images；AlignedGen 进一步说明 DiT 模型中朴素 attention sharing 会带来位置冲突和重复 artifact，需要更精细机制 | 对当前“首帧风格不统一/很 AI”：prompt 层必须固定 Visual Bible、色板、光线、材质、胶片响应和 style reference role；执行层若支持 style reference/共享 attention/IP-Adapter，应把它作为单独资产，而不是每张图自由扩写 |
| AnyI2V | ICCV 2025 运动控制项目 | 高价值控制边界证据；项目页明确指出 T2V 文本 prompt 缺少精确 spatial layout 控制，AnyI2V 用 conditional image + trajectories 做运动控制，并支持 mesh/point cloud 等条件 | Blender/mesh/point cloud/轨迹可作为视频控制输入；复杂空间路线不应靠纯文本猜 |
| Wan-Move | Wan 系运动控制论文/项目 | 高价值运动控制证据；项目页覆盖单物体、多物体、camera control、motion transfer、3D rotation，并展示 linear displacement / dolly in / dolly out | 对历史剧复杂走位/运镜，可把“轨迹”作为控制资产；prompt 只解释戏剧意图、材质和声音 |
| I2VControl-Camera | 论文/相机控制项目 | 高价值运镜控制证据；论文指出相机控制对专业视频很关键，现有方法仍有 precision/subject motion dynamics 限制，并用 camera coordinate point trajectory 提升控制 | 运镜怪不一定是 prompt 失败；需要相机轨迹/强度控制。prompt 应减少为单一 camera intent + endpoint |
| chengzhag/UCPE | GitHub 相机控制论文项目 | 高价值相机控制证据；GitHub API 当前约 199 stars，CVPR 2026，README 明确是 camera-controlled text-to-video generation，支持 camera intrinsics、lens distortion、orientation、camera motion，并提供 PanShot/CameraBench/RealEstate10k、trajectory visualization、pose/lens/orientation demo | 对当前“运镜生成怪”的迁移：prompt 只写戏剧意图和一个主 camera intent；真正的复杂运镜应转成相机轨迹、镜头参数、起始方向、Blender route 或参考运镜视频。它进一步证明运镜稳定是 camera representation/control 问题，不是堆更多运镜词 |
| Direct-a-Video / MotionCtrl / Motion Prompting | 论文/运动控制项目 | 高价值控制理论证据；这些论文共同强调 object motion 与 camera motion 应解耦，文本 prompt 难以表达精细动态动作和时间构图，轨迹/相机 pose/运动参数更可靠 | 后续 skill 的运镜字段应拆成 subject_motion、camera_motion、endpoint、control_asset；复杂镜头进入 trajectory/Blender/pose/control，而不是多写形容词 |
| MotionStream / CamMimic | Adobe/论文运动控制线索 | 中高价值趋势证据；MotionStream 强调交互式运动控制、轨迹/相机控制和实时反馈；CamMimic 强调把参考视频中的 camera motion 迁移到新场景 | 支持“参考运镜视频/路线图”作为素材类型。对当前项目可把 Blender 预演或手工低清参考视频当作 camera motion guide 的未来扩展 |
| CinePreGen | 论文，engine-powered cinematic previsualization | 高价值 Blender/预演旁证；提出 engine-powered diffusion 的 camera/storyboard interface，从全局到局部动态调整 camera；用 multi-masked IP-Adapter 和 engine simulation guidelines 保持一致性，目标就是解决纯文本工作流 camera placement 不足 | 对用户当前工作流：Blender/引擎预演应产出 camera map、storyboard interface 截图、遮罩/角色区域、首尾帧，而不是让最终视频模型猜空间；skill 可输出“需要 Blender blocking”的判定和资产清单 |
| DreamDance | 论文/人体动画控制 | 中高价值人物动作控制证据；用 2D pose 丰富 3D geometry cues、depth/normal maps 提升人物图像动画一致性 | 人物走动/武打/舞动不应只靠文字；需要 pose/depth/normal/驱动视频等控制资产 |
| ComfyGPT / ComfyUI-R1 / ComfySearch | 论文/ComfyUI workflow 自动生成 | 中高价值 workflow 证据；多篇论文都把 ComfyUI workflow 生成视为独立问题，强调节点选择、结构有效性、执行反馈和验证，不适合把本地视频工作流伪装成一条 prompt | 后续 skill 可输出“工作流需求说明”和“节点级控制建议”，但不应承诺纯文案完成本地复杂生成 |
| Hao0321/ai-media-generator | GitHub 跨平台 media skill | 中高价值结构参考；约 149 stars，覆盖多平台 selector、platform-specific references、negative bank、storyboard、browser automation profile。平台版本/停运/benchmark/分辨率等说法变化快，不能直接当事实源 | 可迁移 platform selector、prompt vocabulary bank、negative bank、site profile 分离、“先写对 prompt 再提交”、I2V = motion prompting；具体平台 facts 必须重新复核 |
| wuwangzhang1216/DirectorSKILL | GitHub cinematic director skill | 中高价值导演层参考；约 10 stars，但 README 结构完整，覆盖 script/subtext、beat map、director's book、blocking、shot list、keyframe strategy、tool adapters、QC repair。不是 Seedance 专项 | Blocking before framing、Continuity Bible、director-style overlay、tool adapter、QC diagnostic order；可作为“导演推理层”，再接 Seedance/图片生成执行层 |
| Runway API docs / models guide | 官方 API 文档 | 高价值官方执行源；Markdown 可读。模型页列出 `seedance2`、`seedance2_fast`、`seedance2_mini`、`gen4.5`、`act_two`、`gpt_image_2` 等。Getting Started 确认 I2V/T2V 使用 `promptImage`、`promptText`、`ratio`、`duration`；图像生成支持 `referenceImages` 和 `@tag` 引用 | 可迁移 `@角色/@风格/@场景` reference tagging、任务失败处理、Runway 作为多模型执行层；对 Seedance 仍优先按官方 15s/多模态/首尾帧规则写，不用 Runway 覆盖所有平台 |
| runwayml/skills | Runway 官方 agent skills | 高价值执行工程参考；约 55 stars 但由 Runway 官方文档入口链接。README 和目录确认 generation、integration、upload、API reference、org details、fetch API reference 等技能，覆盖批量生成、轮询、下载、服务端集成、余额/限流查询 | 后续 skill 可借鉴“生成前查 API/余额/限流、上传素材、提交任务、轮询、下载、失败码归因”的执行包；它不是导演 prompt 库，不迁移美学语言 |

## 高星但只做辅助的来源

| 来源 | 类型 | 判断 | 原因 |
|---|---|---|---|
| IgorShadurin/app.yumcut.com | GitHub 高星开源短视频生成器 | 辅助工程参考 | 约 712 stars；README 定位为 TikTok/Reels/Shorts 9:16 流水线，自动 script/voice/visuals/captions/final edit。适合借鉴批量短视频生产、模板和成本控制，不适合历史剧横屏镜头 prompt 或角色一致性规则 |
| ZeroLu/Ultimate-AI-Media-Generator-Skill | GitHub 跨平台媒体生成 skill | 辅助执行参考 | 约 67 stars；支持多模型、quote credits、生成/等待/查余额、图片/视频/音频/音乐；AI Comic Drama workflow 只有 8 scene prompts、2 transitions、character consistency checklist、style lock 的简模板。可借鉴成本预估和任务包，不直接采模型事实或美学 prompt |
| alicejacklead-bit/ai-video-preproduction-workflow | GitHub/Codex skill 前期工作流 | 低星但高相关；降权收录为结构模板 | README 明确链路：创意选题 -> Plan Demo -> 人物形象图 -> 场景图 -> 导演机位图 -> 分镜图 -> 封装图 -> 视频提示词。适合迁移“先固定角色、空间、机位、关键镜头，再交给视频模型”的资产链；星数低，不当成熟项目证明 |
| YouMind-OpenLab/awesome-gpt-image-2 | GitHub prompt 库 | 辅助样本库 | 11500+ prompt，分类好；但方法论弱，主要用于找图片首帧/角色/建筑参考范式 |
| bubblesslayyer-cmd/Awesome-GPT-Image-2-OpenAi | GitHub GPT Image 2 prompt 库 | 辅助样本库；约 68 stars，README 主要是下载/分类入口，方法论很少 | 只抽分类信号：identity retention、multi-panel layout、typography、character/portraiture；不作为规则源 |
| YouMind-OpenLab/awesome-nano-banana-pro-prompts | GitHub prompt 库 | 辅助样本库 | 14600+ prompt，分类体系有价值；不应整库照搬 |
| songguoxs/gpt4o-image-prompts | GitHub prompt 库 | 高星辅助样本库 | 约 3.7k stars，1000+ 案例；可抽取参考图身份锁、JSON 字段、负面 prompt，但内容风格混杂，需按历史剧目标筛选 |
| 0aicoder0/Ultimate-ChatGPT-Image-and-Nano-Banana-Pro-Collection | GitHub prompt 库 | 中星辅助样本库 | 约 462 stars，247 prompts/17 categories；分类清楚，适合抽 image prompt 的 subject/environment/action/style/parameters 结构 |
| GitHub30/Reproducible-Photorealistic-Nano-Banana-Pro-JSON-Prompts | GitHub prompt 库 | 高价值小样本 | 星数低但 JSON 字段很清楚：positive/negative/settings、subject/environment/lighting/camera/negative，可直接映射图片 schema |
| Jermic/awesome-aiart-pics-prompts | GitHub prompt 库 | 辅助社区样本库 | 3000+ 社区案例，来源含 X/小红书/Discord/Reddit；样本多但结构不稳定，适合抽样查创意/类别，不适合作规则源 |
| devanshug2307/Awesome-AI-Image-Prompts | GitHub prompt 库 | 辅助分类库 | 900+ 测试 prompt，类别覆盖 portrait/fashion/architecture/cinematic poster；适合找参考字段和品类，不直接做方法论 |
| ApiMartAI/best-gpt-image-2-prompts | GitHub prompt 库 | 降权辅助 | 星数低、商业导流强；但有 portrait/poster/character sheet/UI mockup 分类，可抽少量 character sheet 写法，不进入核心 |
| LingGuoAI/LingGuo-Drama | GitHub 短剧平台 | workflow 参考 | README 偏产品宣传和工程架构；价值在短剧链路边界，不在 prompt 细节 |
| GongLingRui/ai-video-generation | GitHub AI storyboard platform | workflow 辅助 | 低星，但 README 明确八要素、首尾帧链式生成、33+ 摄影技术库；适合抽结构，不作核心权威 |
| LinHao-city/StoryMind | GitHub 新项目 | 极低星，不能当成熟项目；但 README 架构与 ArcReel/story-shot-agent 互相印证 | StoryboardPlanner、CharacterSheet、SceneConsistencyTracker、PromptEnhancer、先计划再生成 |
| lydiajiancong-dev/videoagent-studio | GitHub 0 星产品原型 | 降权辅助；README 明确是静态原型和 mock 规则，但 QA 维度与本研究互相印证 | 画面质量、动作一致性、镜头稳定性、叙事连贯性、平台适配度；badcase 归因只是概念，不作规则源 |
| showlab/Awesome-Video-Diffusion | GitHub paper list | 辅助论文索引 | 适合找模型/论文，不直接提供 prompt 规则 |
| ai9app/AI-Cinematic-Prompt-Director | GitHub skill/词典 | 降权辅助 | 250+ 摄影词库可查，但星低、平台导流强、容易鼓励堆镜头词；只抽 shot/camera/lighting 分类，不抽风格化输出 |
| PRITHIVSAKTHIUR/LTX-2-LoRAs-Camera-Control-Dolly | GitHub camera-control demo | 专项辅助 | 不是短剧 prompt skill；价值在证明“可控运镜最好选择单一 LoRA/路径”：dolly in/out/left/right、jib up/down、static，比文本堆复杂运镜更稳 |
| aiandcivilization/Img2Vid-Prompts | GitHub I2V 入门指南 | 辅助；内容浅但方向正确 | 强调输入图是起点，视频 prompt 核心写 motion/transformation/style，动作动词、方向、短 clip、相机运动；可用于新人版说明 |
| clipcurator/ai-character-continuity-prompt-library | GitHub 模板库 | 降权辅助；SEO 味重，模板很短 | 可抽字段：stable visual markers、current emotion、relationship state、episode memory、forbidden changes |
| clipcurator/ai-storyboard-character-blocking-shot-list-kit | GitHub 模板库 | 降权辅助；README/模板浅 | 可抽字段：character position、camera placement、movement cue、prompt handoff、continuity risk |
| clipcurator/ai-short-drama-shot-prompt-continuity-kit | GitHub 模板库 | 降权辅助；不是实操案例库 | 可抽字段：shot purpose、camera framing、character anchor、scene state、forbidden drift、generation prompt |
| bagualfilmes-beep/Wan22PromptBuilder_v2 | GitHub Wan prompt builder | 暂不深收；README 太短 | 只作为“本地 Wan/ComfyUI prompt builder 生态存在”的弱信号，暂无可迁移规则 |
| AUTOMATIC1111/stable-diffusion-webui / Fooocus / ComfyUI | 工具平台 | 只作生态背景 | 高星但不是短剧分镜 prompt 库，避免污染核心研究 |
| Pika 官网 + Fal API pages | 官方站点 + 第三方执行入口 | 辅助执行证据；不是 Pika 官方 prompt 指南 | Pika 官网 API 页明确导向 Fal.ai；Fal `fal-ai/pika/v2.2/image-to-video` 页面可读 request schema：`image_url` 为第一帧、`prompt` 必填、`seed`、`negative_prompt`、`resolution`=`720p/1080p`、`duration`=`5/10`。同页 function list 还能读到 `pikascenes`、`pikaframes`、`pikaffects`、`pikaswaps` 等 endpoint。迁移为执行 adapter 线索：Pika 更适合短 I2V、keyframes/scenes/effects；不要把这些字段写成官方美学 prompt 规则 |
| Fal Kling 2.0 Master I2V page | 第三方 Kling 执行入口 | 辅助执行证据；Kling 官方参数仍需复核 | Fal `fal-ai/kling-video/v2/master/image-to-video` 页面可读 request schema：`prompt`、`image_url` 必填，`duration`=`5/10`，`negative_prompt` 默认 `blur, distort, and low quality`，`cfg_scale` 默认 0.5 且范围 0-1。页面 function list 还出现 Kling V2.6 standard Motion Control：参考视频负责动作、参考图负责外观，并区分 orientation=`video` 适合复杂动作 max 30s、orientation=`image` 适合镜头移动 max 10s。迁移为“执行字段/控制资产”线索，不当 Kling 官方 prompt 指南 |
| Fal Wan FLF2V page | 第三方 Wan 执行入口 | 弱辅助执行证据 | Fal `fal-ai/wan-flf2v` 页面可稳定读到页面描述：Wan-2.1 first-last-frame-to-video 会把给定 first frame 与 desired end frame 桥接为平滑连贯运动，endpoint 为 `https://fal.run/fal-ai/wan-flf2v`。当前没有稳定读到完整 request schema，因此只迁移“首尾帧控制路线”这一结论，不写具体字段 |
| OpenAI Sora Help Center / Sora official redirect | 官方站点 | 历史/弃用状态的产品形态参考 | OpenAI Help 当前可读，但要同时标注适用范围：`openai.com/sora/` 跳转到 discontinuation 页面，写明 Sora web/app experiences 已在 2026-04-26 discontinued，Sora API 将在 2026-09-24 discontinued；`Generating videos on Sora` 页面只适用于 Sora 1 web，不适用于 Sora app 或 Sora 2，并写明 Sora 1 web 正在 deprecated。后续不把 Sora 写成当前执行平台，只迁移 storyboard card spacing、re-cut、remix、blend、loop 这类产品概念 |
| Runway Help Center | 官方站点 | 暂不作为证据 | Runway API docs 已可读并单独收录；Runway Help Center 当前仍受 Cloudflare challenge 影响，不能把 Help Center prompt 内容当已读资料。后续需要浏览器人工/可访问官方源复核 |

## 降权或暂不收录

| 来源 | 原因 |
|---|---|
| 泛 awesome 列表 | 星数极高但主题不聚焦，几乎不能直接转成 director skill |
| 只有示例图无 prompt 结构的图库 | 只能做审美参考，不能做规则 |
| 只强调“viral/hook”但无失败修复机制的短视频 prompt | 与历史剧真实风格目标冲突，除非有明确镜头/节奏规则 |
| kanupriya-GuptaM/T2V-Failures-ai-Response | 搜索摘要很像 failure taxonomy，但 README 404，暂不收录；后续若内容可读再复核 |
