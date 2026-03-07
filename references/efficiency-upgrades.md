# Efficiency Upgrades (memU-inspired)

These patterns are adapted for lightweight file-based memory systems.

## 1) Retrieval as a pipeline

Do not run one giant search by default.

Recommended stages:
1. L1 quick lookup (today/task/keyword-frequency)
2. L2 cross-day memory lookup (`memory/*.md`)
3. L3 chat archive fallback (`agent-memory/chat/*`)

Add a sufficiency gate between stages. Stop early if evidence is enough.

## 2) Scope guard

Always decide scope first:
- who (user/project/session)
- when (date range)
- where (daily memory vs chat archive)

Narrow scope reduces noise and improves retrieval speed.

## 3) Reference-friendly write-back

For high-impact entries, add `RefID` in daily memory.

Example:

```markdown
- RefID: D-2026-03-07-03
- Decision: ...
- Verify: memory/2026-03-07.md#L120
```

This makes later answers citeable and auditable.

## 4) Reinforcement and compaction

If the same preference/rule appears repeatedly:
- reinforce once in daily memory,
- then promote to stable layer (`USER.md` or `agent-memory/common-info.md`).

Avoid repeating the same long explanation in every daily note.

## 5) Confidence-aware output

When evidence is weak:
- explicitly mark low confidence,
- show what was checked,
- list the next retrieval step.

This prevents false certainty and improves trust.

## 6) `/new`-resilient checkpoint mode

For high context-reset frequency, add lightweight checkpoints:
- **Mandatory** before `/new` / `/reset` / context refresh.
- **Mandatory** on key state changes (new preference, task state change, key external action).
- **Optional** periodic fallback every 30–45 minutes.
- Add min-interval guard to avoid noise (e.g., skip if last checkpoint < 25 minutes).

Minimal checkpoint payload:
- timestamp
- active task snapshot (`在做/就绪/中断`)
- optional top active task IDs

This preserves recoverability without forcing heavy full summaries each time.
