# Action Process Shot

Use for running, crossing space, fighting beat, reaching, grabbing, pushing, falling, lifting, turning, escaping, or any physical action with a visible process.

## Purpose

The shot should show start pose, action path, contact/change, and endpoint without teleporting or muddy motion.

## Required Prompt Details

- Define start pose and exact screen direction.
- Describe the route through space: left-to-right, toward camera, away from camera, around table, through doorway.
- Name contact points and physics: hand on shoulder, foot on step, fabric dragged, object weight.
- Use one main camera movement that supports the path.
- For complex action, split into multiple clips instead of stacking actions.
- End with a stable pose or completed contact.

## Avoid

- Teleporting, flying, sliding feet, wall/table collision, extra limbs.
- Multiple simultaneous actions that compete.
- Sudden camera cuts inside the same prompt.
- Open-ended motion with no endpoint.

## Useful Structure

```text
00:00-00:02 start pose and route are clear.
00:02-00:05 action begins; body weight shifts naturally.
00:05-00:09 contact/change occurs.
结尾：完成动作后停住 1 秒。
```
