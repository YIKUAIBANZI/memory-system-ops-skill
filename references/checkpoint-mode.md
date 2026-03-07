# Checkpoint Mode (`/new`-resilient)

Use this mode when context resets are frequent and you want reliable recovery.

## Trigger Matrix

### 1) Event trigger (required)
Append checkpoint immediately when any of these happens:
- new user preference or boundary
- task status changes (`在做/就绪/中断`)
- key external action completed (submission, delivery, publish, etc.)

### 2) Pre-refresh trigger (required)
Before `/new`, `/reset`, or manual context refresh:
- append a lightweight checkpoint first

### 3) Periodic fallback (optional)
If session is active for long periods:
- run every 30–45 minutes
- enforce min-interval guard (e.g., skip if last checkpoint < 25 minutes)

## Minimal Checkpoint Structure

```markdown
- RefID: AUTO-YYYYMMDD-XX
- Decision: 自动检查点落盘（auto flush checkpoint）。
- Why: 在上下文切换前保持状态连续。
- Impact: /new 后可快速恢复任务和偏好。
- Next: 持续执行事件写回 + 检查点策略。
- Verify:
  - Checkpoint time: YYYY-MM-DD HH:mm:ss
  - TASKS snapshot: 在做=N, 就绪=M, 中断=K
  - 当前在做: T-xxx(...); T-yyy(...)
```

## Noise Control

- Do **not** append identical large summaries repeatedly.
- If no material changes, keep checkpoints short (timestamp + task snapshot only).
- Prefer evidence links (`RefID` / file path) over repeated prose.

## Recommended Operating Order

1. Write event-driven updates first.
2. Write checkpoint before context reset.
3. Update keyword-frequency after write-back.
4. Compact repeated rules into stable layers (`USER.md` / `common-info.md`).
