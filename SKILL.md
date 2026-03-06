---
name: memory-system-ops
description: Operate a fast and reliable personal memory system using layered notes, structured daily entries, task board discipline, keyword frequency tracking, and retrieval fallback rules. Use when setting up or maintaining memory files, debugging "you forgot" issues, improving recall speed/accuracy, or enforcing daily write-back and task archive workflows.
---

# Memory System Ops

Run this skill when the user asks to improve, audit, or maintain memory quality.

## Strict Runbook (for weaker agents)

When this skill is triggered, execute in this order:

1. Read current state files:
   - `USER.md`
   - `TASKS.md`
   - `memory/YYYY-MM-DD.md` (today)
2. If user asks about past facts, use retrieval fallback `L1 -> L2 -> L3` before answering.
   - If the question is keyword-driven (e.g. “高频词/关键词/这个词出现几次/聊天记录里提过吗”), do keyword-first lookup:
     1) check `memory/keyword-frequency.md`,
     2) then scan `memory/*.md` for context,
     3) then fallback to `agent-memory/chat/*` if still low confidence.
3. For every important new decision, append one structured block:
   - `Decision / Why / Impact / Next / Verify`
4. Update `TASKS.md` with only `在做 / 就绪 / 中断`.
   - If `中断`, must fill `Blocked by` and `Resume step`.
5. After memory write-back, update `memory/keyword-frequency.md`.
6. Reply with verifiable evidence (file path + what changed), not vague claims.

Never skip step 2 for historical recall questions.

## Core Model

Use 3 layers:
- `USER.md`: stable user profile and long-term preferences.
- `memory/YYYY-MM-DD.md`: daily facts and decisions.
- `TASKS.md`: active executable tasks only (`在做/就绪/中断`).

Use `TASKS_ARCHIVE/YYYY-MM.md` for completed tasks.

## Daily Write-Back Standard

For key events, write:
- `Decision`
- `Why`
- `Impact`
- `Next`
- `Verify`
- optional `Tags`, `Aliases`

When the user reports memory mismatch ("你没记住"), immediately show the exact stored location and verify against file content.

## Retrieval Fallback Order

1. L1: today memory + task board
2. L2: `memory/*.md`
3. L3: `agent-memory/chat/daily -> weekly -> monthly` (when low confidence)

### Keyword-first rule (must apply)

For keyword questions, do this before final answer:
1. Read `memory/keyword-frequency.md` for candidate terms and counts.
2. Locate matching daily entries in `memory/*.md`.
3. Return concrete path evidence (where the keyword/decision appears).

Always prefer verified answers over guessed answers.

## Task Board Rules

- Keep only active states in `TASKS.md`: `在做/就绪/中断`.
- For `中断`, require:
  - `Blocked by`
  - `Resume step`
- Move completed tasks to `TASKS_ARCHIVE/YYYY-MM.md`.

## Rule Placement

- Keep user identity and stable preferences in `USER.md`.
- Keep operational rules (migration/archive/retrieval) in `agent-memory/common-info.md`.

## Keyword Tracking

- Maintain keyword seeds manually in `memory/keyword-seeds.md`.
- Generate stats to `memory/keyword-frequency.md` after each daily write-back.
- Event-driven update only (no periodic cron requirement).

## References

- Workflow details: `references/workflow.md`
- Daily template: `references/daily-template.md`
- Task board template: `references/tasks-template.md`
