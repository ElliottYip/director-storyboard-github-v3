# Sleeve / Occlusion Transition

Use for: 水袖扫过、袖子遮挡、门关上、雾遮挡、水花遮挡。

## Required Controls

- Occluding object and direction.
- What is hidden.
- Endpoint filled frame or partial wipe.
- Next shot bridge.

## Template

```text
## 动作提示词
00:00-00:02 [角色/物体] 开始从 [方向] 进入前景。
00:02-00:05 [袖子/门/水花/雾] 横向/纵向扫过，遮住 [人物/背景]。
00:05-00:08 遮挡物占据画面 [比例]，形成自然剪辑点。

## 转场锚点
遮挡桥：上一镜的 [遮挡物] 填满画面，下一镜从 [相似颜色/材质/声音] 开始。

## 负面
不要旋转镜头，不要突然变焦，不要让遮挡物变成抽象图案，不要文字。
```
