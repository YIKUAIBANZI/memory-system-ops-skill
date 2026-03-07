# Workflow

## 0) Trigger

Use this workflow when the user asks to:
- improve memory quality,
- check historical facts,
- fix "you forgot" issues,
- or maintain memory/task files.

## 1) Read state (mandatory)

1. `USER.md`
2. `TASKS.md`
3. `memory/YYYY-MM-DD.md`
4. optional: `agent-memory/common-info.md`

## 2) Set retrieval scope (efficiency guard)

Before searching, define scope if possible:
- user / project / topic
- date range
- session/source

Narrow scope first, broaden only when confidence is low.

## 3) Retrieve with staged gates (L1 -> L2 -> L3)

### L1 (fast path)
- check today memory + `TASKS.md`
- if keyword query, check `memory/keyword-frequency.md`
- if evidence is enough, stop

### L2 (cross-day memory)
- scan `memory/*.md` for matching decisions/evidence
- run sufficiency check again
- if enough, stop

### L3 (fallback only)
- scan `agent-memory/chat/daily -> weekly -> monthly`
- use only when L1/L2 are insufficient

## 4) Write-back structured decision

Append key updates to `memory/YYYY-MM-DD.md`:
- `Decision`
- `Why`
- `Impact`
- `Next`
- `Verify`
- optional: `RefID`, `Scope`, `Confidence`, `Tags`, `Aliases`

## 5) Maintain task board

Update `TASKS.md` with active states only:
- `在做`
- `就绪`
- `中断`

If `中断`, must fill:
- `Blocked by`
- `Resume step`

Completed tasks -> `TASKS_ARCHIVE/YYYY-MM.md`.

## 6) Update keyword stats (event-driven default)

After daily write-back:
- run keyword stats update
- refresh `memory/keyword-frequency.md`

## 7) Apply checkpoint policy (`/new`-resilient mode)

- Event trigger: append immediately on new preference/boundary, task state change, or key external action completion.
- Pre-refresh trigger: before `/new` / `/reset` / context refresh, append lightweight checkpoint first.
- Optional periodic fallback: every 30–45 minutes with min-interval guard (e.g., skip when last checkpoint < 25 minutes).
- If no new decisions, still append minimal checkpoint (timestamp + TASKS snapshot).

## 8) Compact repeated rules

If the same decision/rule appears >=3 times:
- promote long-term preference to `USER.md`
- promote process rule to `agent-memory/common-info.md`
- keep source references for traceability

## 9) Response format

Return with evidence:
- what changed
- exact file path(s)
- verification context
- confidence note when uncertainty remains
