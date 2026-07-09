# First-Frame Histogram And Face Integrity

Use when the user gives histogram/color analysis for a first frame, scene reference, or visual bible.

## Purpose

Histogram constraints should stabilize style and lighting. They must not force the model to crush faces, distort identity, or make eyes/mouth unreadable.

## Prompt Block

```text
## 首帧直方图 / 色板约束
首帧直方图目标：低调建筑参考，暗部 65-68%，中间调 22-25%，高光 8-12%。
关键色板：#141516, #232524, #2D2F2E, #3F4240, #4D4D4B, #67696B, #B8BBC1。
使用低饱和灰绿色寺庙建筑、旧木、灰瓦、石阶、阴天雾面光。
黑位轻微抬升，避免死黑；避免明亮干净的旅游照光线。
注意：直方图主要约束建筑、环境、雨雾、木石材质和整体调色，不要把人物脸压进死黑，不要让低调光影造成脸部扭曲。
```

## Face Integrity Block

```text
## 脸部稳定 / 身份保护
人物脸部继承角色参考图，保持脸型、年龄、五官比例、头颅结构和表情方向稳定。
脸部可以处在柔和阴影中，但必须保留自然中间调、眼睛高光、鼻梁和嘴唇轮廓。
不要脸部拉伸、歪斜、压扁、融化、变成面具；不要眼睛大小不一；不要嘴巴漂移；不要因为低调光影把五官压成黑洞。
如果人物脸部在山门阴影中，脸比环境略亮半档，保持可读的哑光皮肤和轻微毛孔。
```

## When Face Distortion Appears

Retake by changing only one variable:

```text
保留原动作、构图、服装和参考图不变。只新增脸部稳定规则：人物脸部保持参考图脸型和五官比例，低调光影只作用于环境和衣物，脸部保留柔和中间调，不要扭曲、拉伸、压暗、融化。
```
