# Two Character Support Shot

Use for: 扶住、托住、护退、支撑、保护、拉走、二人身体接触。

## Required Controls

- Bind which character supports and which character is supported.
- Define contact points: shoulder, upper arm, back, sleeve shield.
- Keep identities and costumes separate.
- If contact deforms, upgrade to pose/depth/canny or split shot.

## Template

```text
## 人物状态
[支撑者] 在 [位置]，用 [手/袖子] 支撑 [被支撑者] 的 [肩/背/手臂]。
[被支撑者] 位于 [低处/前景/后景]，身体 [低伏/跪步/站立不稳]，但不出现禁忌姿态。

## 动作提示词
00:00-00:02 [被支撑者] 失去力量/停住，[衣袖/手] 落到 [地面/水中]。
00:02-00:05 [支撑者] 从 [方向] 进入，手落在 [接触点]，另一只袖子形成保护。
00:05-00:08 两人一起向 [方向] 移动/停住，形成稳定支撑端点。

## 身份保护
保持两人身份分离：[A] 只继承 [A reference]；[B] 只继承 [B reference]。不要交换服装，不要融合脸。
```
