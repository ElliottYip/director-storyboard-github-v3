# Camera Language

Video models often invent camera motion. Specify one main movement only.

## Camera Fields

```text
景别：远景 / 中远景 / 中景 / 近景 / 特写
机位：平视 / 低角度 / 高位俯视 / 门内向外 / 过肩
焦段感：24mm 大远景 / 35mm 历史剧中景 / 50mm 人物压缩 / 85mm 特写
主运动：固定 / 缓慢推近 / 缓慢后拉 / 稳定跟拍 / 轻微横移
禁止：环绕、飞行、突然变焦、快速横移、剧烈手持、切镜头
```

## Stable Defaults

- Entrance: fixed frontal medium-wide, slow push-in.
- Door separation: static split-depth frame.
- Flood wide: high wide static, subtle forward drift.
- Support/contact: medium shot, static or slow tracking backward.
- Occlusion transition: stable camera; sleeve/object provides motion.

## Rule

If subject motion is complex, camera should be static. If camera moves, subject motion should be simple.
