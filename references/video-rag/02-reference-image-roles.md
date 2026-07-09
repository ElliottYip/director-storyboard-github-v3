# Reference Image Roles

Every image must have one primary role. Do not attach images as vague "references".

## Role Wording

```text
Image1：角色身份 only。锁定脸、年龄、体型、服装、发型。不要复制背景、姿势、光线。
Image2：场景环境 only。锁定建筑、材质、空间层级、入口/出口。不要复制人物。
Image3：首帧 lock。锁定开场构图、人物位置、起始姿势、光影和调色。
Image4：分镜顺序 storyboard_order。锁定动作顺序、运动方向、终点姿态。身份以角色图为准。
```

## Forbidden Transfer Examples

- Four-view sheet: do not transfer panel layout, gray studio background, neutral pose.
- Style reference: do not transfer face, costume, exact scene.
- Environment reference: do not transfer accidental people.
- Action reference: do not transfer unwanted camera crop if a new composition is required.

## Multi-Character Binding

Always bind positions:

```text
白素贞：前景左侧/低处/白衣/头面清晰。
小青：后景右侧/青衣/护卫动作。
法海：高处石阶中央/僧袍/压阵。
许仙：门内暗处/青灰书生衣/手在门缝。
```

## Common Failure Fix

If identity/costume merges:

```text
保持两人身份完全分离：白素贞只继承 Image1 的白衣和头面；小青只继承 Image2 的青衣和脸。不要交换服装，不要融合脸，不要复制对方头饰。
```
