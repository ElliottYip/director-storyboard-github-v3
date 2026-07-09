# Lighting Color Material

Use measurable visual constraints, not vague "cinematic".

## Histogram Block

```text
低调灰绿色雨雾历史剧质感。暗部约 55-68%，中间调约 22-35%，高光约 8-12%。
黑位轻微抬升，有褪色胶片黑，不要死黑。
高光柔和滚降，不要硬曝，不要 HDR glow。
```

## First-Frame Histogram Block

When a first frame or scene reference has histogram analysis, output it as a separate block:

```text
首帧直方图目标：暗部 [x%]，中间调 [x%]，高光 [x%]。
关键色板：[hex list]。
该直方图只约束整体建筑、环境、雨雾和光影；人物脸部必须保持清楚、自然、稳定，不要因为低调光影导致脸部扭曲或五官压暗变形。
黑位轻微抬升，避免死黑；脸部保留柔和中间调和可读轮廓。
```

## Material Block

```text
湿木门板有粗糙木纹和铁钉水痕；旧石阶有泥水和苔痕；泥水浑浊、有真实水花重量。
白衣为湿丝绸、薄纱水袖、刺绣线凸起、珍珠银饰头面。
青衣为湿青绿色丝绸，衣摆带泥水重量。
僧袍为粗布湿重，不要金属盔甲感。
皮肤为哑光妆面或自然哑光皮肤，有轻微毛孔，不要油光、蜡像、塑料脸。
```

## Face Integrity Under Low-Key Light

```text
低调光影不能破坏脸部结构。保持脸型、五官比例、眼睛对称、鼻梁自然、嘴巴闭合位置稳定。脸部不能被阴影挤压、拉伸、扭曲、融化、变成面具；不要为了暗调把眼睛和嘴巴压成黑洞。人物脸部允许比环境略亮一点，保留自然中间调和皮肤细节。
```

## Avoid

- 舞台追光 unless stage style is desired.
- 霓虹蓝绿法术光.
- 鬼片闪光.
- 旅游照晴天高饱和.
- 过锐 HDR.
