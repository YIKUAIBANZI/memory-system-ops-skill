# Daily Decision Template

- Decision:
- Why:
- Impact:
- Next:
- Verify:
- RefID: (optional, short stable id for citation)
- Scope: (optional, e.g. user/project/date range)
- Confidence: (optional, high/medium/low)
- Tags: (optional)
- Aliases: (optional)

## Lightweight Checkpoint Template (`/new` mode)

- RefID: AUTO-YYYYMMDD-XX
- Decision: 自动检查点落盘（auto flush checkpoint）。
- Why: 在上下文切换前保持状态连续。
- Impact: /new 后可快速恢复任务和偏好。
- Next: 继续执行事件写回 + 定时检查点策略。
- Verify:
  - Checkpoint time: YYYY-MM-DD HH:mm:ss
  - TASKS snapshot: 在做=N, 就绪=M, 中断=K
  - 当前在做: T-xxx(...); T-yyy(...)
