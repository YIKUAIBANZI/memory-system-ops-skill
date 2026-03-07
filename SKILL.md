---
name: memory-system-ops
description: Operate a fast and reliable personal memory system using layered notes, structured daily entries, task board discipline, keyword frequency tracking, retrieval fallback rules, and evidence-first answers. Use when setting up or maintaining memory files, debugging "you forgot" issues, improving recall speed/accuracy, or enforcing daily write-back and task archive workflows.
---

# Memory System Ops

Run this skill when the user asks to improve, audit, or maintain memory quality.

## Strict Runbook (for weaker agents)

When this skill is triggered, execute in this order:

1. Read current state files:
   - `USER.md`
   - `TASKS.md`
   - `memory/YYYY-MM-DD.md` (today)
   - optional: `agent-memory/common-info.md`
2. Set retrieval scope before searching (when applicable):
   - user / project / session / date range
   - avoid global scans if a narrower scope is enough.
3. Retrieve with efficiency gates (must follow):
   - **L1**: today memory + task board (+ `memory/keyword-frequency.md` for keyword questions)
   - run a sufficiency check: if answer is already supported by evidence, stop here
   - **L2**: scan `memory/*.md` only when L1 is insufficient
   - run a sufficiency check again
   - **L3**: fallback to `agent-memory/chat/daily -> weekly -> monthly` only when confidence remains low
4. For every important new decision, append one structured block:
   - `Decision / Why / Impact / Next / Verify`
   - optional but recommended: `RefID / Scope / Confidence / Tags / Aliases`
5. Update `TASKS.md` with only `在做 / 就绪 / 中断`.
   - If `中断`, must fill `Blocked by` and `Resume step`.
6. After memory write-back, update `memory/keyword-frequency.md`.
7. Apply checkpoint policy (for `/new`-resilient mode):
   - **Event trigger (mandatory):** on new preference/boundary, task state change, or key external action completion, append write-back immediately.
   - **Pre-refresh trigger (mandatory):** before `/new` / `/reset` / context refresh, append a lightweight checkpoint first.
   - **Periodic fallback (optional but recommended):** every 30–45 minutes, append a lightweight checkpoint with a min-interval guard (e.g., skip if last checkpoint < 25 minutes).
   - If no new decisions, still write a minimal checkpoint (timestamp + TASKS snapshot).
8. If the same rule/decision repeats >=3 times, compact and promote:
   - long-term preference -> `USER.md`
   - operational rule -> `agent-memory/common-info.md`
   - keep a reference link to original daily entries.
9. Reply with verifiable evidence (file path + what changed), not vague claims.

Never skip step 3 for historical recall questions.

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
- optional: `RefID`, `Scope`, `Confidence`, `Tags`, `Aliases`

When the user reports memory mismatch ("你没记住"), immediately show the exact stored location and verify against file content.

## Checkpoint Standard (frequent `/new` support)

Use this lightweight checkpoint block when there is no major new decision but state continuity is needed:
- `RefID` (e.g., `AUTO-YYYYMMDD-XX`)
- `Decision` (auto flush checkpoint)
- `Why` (protect continuity before reset/switch)
- `Impact` (recoverable context after reset)
- `Next` (continue normal write-back loop)
- `Verify`:
  - checkpoint time
  - `TASKS.md` snapshot counts (`在做/就绪/中断`)
  - optional top active task IDs

Goal: preserve recoverability without bloating daily notes.

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

## Efficiency Enhancements (memU-inspired)

1. **Pipeline stages, not one-shot search**
   - Run retrieval as staged execution (L1/L2/L3 + sufficiency checks).
2. **Scope guard first**
   - Narrow `where` scope before retrieval to reduce noise and latency.
3. **Sufficiency gate**
   - Stop retrieval once evidence is enough; avoid expensive unnecessary scans.
4. **Reference-friendly write-back**
   - Add short `RefID` in important entries so later answers can cite quickly.
5. **Reinforcement + compaction**
   - Repeated decisions should be promoted to stable layers to prevent daily-file bloat.

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
- Default mode is event-driven.
- For high-frequency `/new` workflows, you may enable optional periodic checkpoint mode; if enabled, keep a min-interval guard to avoid noisy duplicate entries.

## References

- Workflow details: `references/workflow.md`
- Daily template: `references/daily-template.md`
- Task board template: `references/tasks-template.md`
- Efficiency notes: `references/efficiency-upgrades.md`
- Checkpoint mode: `references/checkpoint-mode.md`
