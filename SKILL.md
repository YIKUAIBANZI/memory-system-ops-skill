---
name: memory-system-ops
description: Operate a fast and reliable personal memory system using layered notes, structured daily entries, task board discipline, keyword frequency tracking, and retrieval fallback rules. Use when setting up or maintaining memory files, debugging "you forgot" issues, improving recall speed/accuracy, or enforcing daily write-back and task archive workflows.
---

# Memory System Ops

Run this skill when the user asks to improve, audit, or maintain memory quality.

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
