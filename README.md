# memory-system-ops

A practical AgentSkill for operating a **fast + accurate + verifiable** personal memory system.

This skill is designed for assistants that need to:
- keep daily memory clean and queryable,
- prevent "you forgot" failures,
- maintain a strict active task board,
- and evolve memory rules without polluting user profile files.

---

## What this skill solves

Typical long-running assistant problems:
1. Facts are written, but hard to retrieve quickly.
2. Rules and profile information are mixed together.
3. Task boards grow forever and lose signal.
4. Interrupted tasks are hard to resume.
5. Memory quality degrades after many context resets.

`memory-system-ops` addresses these with a layered model, strict write-back structure, retrieval fallback order, and task archive discipline.

---

## Core design

### 1) Layered memory model

- `USER.md` → stable user profile & long-term preferences
- `memory/YYYY-MM-DD.md` → daily facts & decisions
- `TASKS.md` → active executable tasks only (`在做 / 就绪 / 中断`)
- `TASKS_ARCHIVE/YYYY-MM.md` → completed task archive

### 2) Structured daily write-back

For key items, always write:
- `Decision`
- `Why`
- `Impact`
- `Next`
- `Verify`
- optional: `Tags`, `Aliases`

### 3) Retrieval fallback order

- **L1**: today memory + task board
- **L2**: `memory/*.md`
- **L3**: `agent-memory/chat/daily -> weekly -> monthly` (when confidence is low)

### 4) Task board discipline

- Keep only active states in `TASKS.md`
- For `中断`, require:
  - `Blocked by`
  - `Resume step`
- Move completed tasks to `TASKS_ARCHIVE/YYYY-MM.md`

### 5) Rule placement

- Put user identity/preferences in `USER.md`
- Put operational rules in `agent-memory/common-info.md`

---

## Repository structure

```text
memory-system-ops/
├── SKILL.md
├── README.md
└── references/
    ├── workflow.md
    ├── daily-template.md
    └── tasks-template.md
```

---

## Included references

- `references/workflow.md`
  - End-to-end operational workflow
- `references/daily-template.md`
  - Structured daily memory template
- `references/tasks-template.md`
  - Active task board schema and archive rules

---

## Quick start

### Option A: Use as a local skill folder

1. Copy this folder into your skills directory.
2. Ensure `SKILL.md` is discoverable by your agent runtime.
3. Start using the workflow in your daily operations.

### Option B: Clone from GitHub

```bash
git clone https://github.com/YIKUAIBANZI/memory-system-ops-skill.git
```

Then place or symlink it into your agent skills path.

---

## Suggested operating loop

1. Read current context (`USER.md`, `TASKS.md`, today memory, common-info)
2. Execute task / respond to user
3. Write structured decision to daily memory
4. Update task board state
5. Archive completed tasks
6. Run keyword tracking update (event-driven)

---

## Example: handling "you forgot"

When user says "你没记住":
1. Do not argue.
2. Retrieve with fallback order (L1→L2→L3).
3. Show exact stored location.
4. If missing, append a structured correction entry immediately.

This keeps trust high and avoids repeated memory drift.

---

## Compatibility notes

This skill assumes the following files (or equivalents) exist:
- `USER.md`
- `TASKS.md`
- `memory/`
- optional but recommended: `agent-memory/chat/*`, `agent-memory/common-info.md`

If your project uses different paths, keep the same logic and update references accordingly.

---

## Maintenance tips

- Keep `USER.md` concise and stable.
- Avoid writing operational policy directly into profile files.
- Keep `TASKS.md` short (active items only).
- Treat `Verify` as evidence-based closure, not prose.
- Prefer small, frequent write-backs over large delayed summaries.

---

## Versioning recommendation

Use semantic versioning for this skill:
- `v0.x` for rapid iteration
- `v1.0` when workflow is stable across multiple sessions/projects

Suggested release checklist:
- [ ] Validate SKILL structure
- [ ] Test retrieval fallback behavior
- [ ] Test task archive workflow
- [ ] Confirm no path assumptions break target environment

---

## License

Add your preferred license if needed.
