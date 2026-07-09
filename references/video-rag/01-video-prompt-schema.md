# Final Video Prompt Schema

Output Chinese prompts by default when the project is Chinese.

```text
## 参考图使用
Image1：作为 [角色/场景/首帧/分镜] 参考。锁定：[...]. 禁止迁移：[...].

## 风格提示词
[媒介 + 时代/类型 + palette + material + skin + forbidden look]

## 首帧直方图 / 色板约束
[首帧或场景参考图的 shadows/midtones/highlights 比例、关键色板、黑位/白位规则。说明该约束只控制整体光影和场景，不得压坏人物脸。]

## 人物状态
[谁在画面中、位置、意图、情绪、身体状态、必须保持的姿态]

## 动作提示词
00:00-00:02 ...
00:02-00:05 ...
00:05-00:08 ...
结尾：保持 [endpoint pose] 1 秒，形成可剪切静帧。
明确禁止：[最容易错误出现的动作]

## 表演 / 微表情
眼神：[...]
眉眼：[...]
嘴巴：[闭合/不说话/轻微呼吸]
呼吸/停顿：[...]

## 脸部稳定 / 身份保护
保持参考图人物的脸型、五官比例、头颅结构和表情方向稳定。脸部不能被暗部压变形，不能拉伸、歪斜、塌陷、融化、变成面具。眼睛大小对称，鼻梁和嘴巴位置自然，面部边缘清楚但不过锐。

## 运镜镜头提示词
景别：[...]
机位：[...]
焦段感：[...]
主运动：[只允许一个]
禁止：[环绕/突然变焦/快速横移/抖动/切镜]

## 光影材质提示词
光线：[...]
黑位/高光：[...]
材质：[cloth/wood/stone/water/skin physics]

## 音效提示词
无台词/有台词：[...]
环境声：[...]
物体声：[...]
禁止音频：[...]

## 转场锚点
[声音/动作/视线/物体/光线/遮挡/水流] 连接到 [next clip].

## 负面提示词
[text/watermark/identity/costume/camera/material/sound/action negatives]
```

## Required Endpoint

Every prompt must specify an endpoint:

- standing still
- gate nearly closed
- sleeve occlusion fills frame
- water recedes and holds
- gaze holds in a cuttable pose

If no endpoint is obvious, invent a simple one. Do not end with open-ended motion.
