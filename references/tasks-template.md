# TASKS board schema

| 状态 | 任务ID | 任务 | 说明 | 下一步 | Blocked by | Resume step |
|---|---|---|---|---|---|---|
| 在做/就绪/中断 | T-xxxx | ... | ... | ... | ... | ... |

Rules:
- Only `在做/就绪/中断` in active board.
- If `中断`, fill both `Blocked by` and `Resume step`.
- On completion, move task to `TASKS_ARCHIVE/YYYY-MM.md`.
