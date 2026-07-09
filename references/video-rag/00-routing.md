# Video RAG Routing

Use deterministic keyword/tag routing. Load `01-video-prompt-schema.md` for all final video prompts, then load one or two shot templates.

## Shot Type Routes

| User / script signal | Load |
|---|---|
| 开场, 建立空间, 空镜, 环境, 地理关系, 场景介绍, 外景, 内景, 时间地点 | `examples/establishing-location-shot.md` |
| 独处, 反应, 意识到, 忍住, 沉默, close-up, 特写, 情绪变化, 微表情 | `examples/single-character-performance-shot.md` |
| 对话, 听见, 回答, 对视, 争吵, 试探, 沉默对峙, shot reverse shot | `examples/dialogue-reaction-shot.md` |
| 物件, 道具, 线索, 手部, 拿起, 放下, 插入镜头, 特写物, prop reveal | `examples/object-insert-shot.md` |
| 追逐, 打斗, 摔倒, 跑, 抓, 推, 拉, 跨越, 转身, 抬手, 动作过程 | `examples/action-process-shot.md` |
| 转场, match cut, 声音桥, 视线桥, 动作桥, 光线桥, 物体桥 | `examples/generic-transition-shot.md` |
| 走出, 入场, 缓步, 三步, 定身, 亮相, 站住, 镇守 | `examples/standing-character-entrance.md` |
| 门, 门缝, 隔门, 困住, 相望, 关闭, 山门合 | `examples/door-separation-shot.md` |
| 洪水, 水漫, 起水, 法术, 大远景, 水浪, 江水, 规模 | `examples/water-vfx-wide-shot.md` |
| 扶住, 搀扶, 护退, 保护, 托住, 拉走, 二人接触 | `examples/two-character-support-shot.md` |
| 僧众, 群僧, 队形, 列阵, 半弧阵, 群演 | `examples/group-blocking-shot.md` |
| 遮挡, 水袖扫过, 袖子擦过, 雾中消失, 转场 | `examples/sleeve-occlusion-transition.md` |

## Always Load When Needed

- Reference image confusion or multi-image input: `02-reference-image-roles.md`
- Action too vague or wrong action appears: `03-action-microexpression.md`
- Weird camera movement: `04-camera-language.md`
- Fake material / bad light / oily skin: `05-lighting-color-material.md`
- Weird sound / platform auto-audio: `06-sound-negative-rules.md`
- Adjacent clips feel hard cut: `07-transition-builder.md`
- Retake or diagnosis: `08-retake-diagnosis.md`

## Combination Rules

- Combine at most two shot templates plus the generic schema.
- Prefer generic templates first. Use specialized templates only when a specific visual problem is present, such as doors, water VFX, group formations, sleeve occlusion, or two-person body support.
- If one shot includes entrance + group blocking, make the main subject's entrance dominant and group blocking secondary.
- If the action is complex contact, prefer splitting into two clips or using pose/depth/canny over adding more prose.
- If the final prompt exceeds platform comfort, remove optional description before removing action path, camera, or negatives.
