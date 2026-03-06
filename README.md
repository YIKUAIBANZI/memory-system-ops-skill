# memory-system-ops

[![GitHub stars](https://img.shields.io/github/stars/YIKUAIBANZI/memory-system-ops-skill?style=social)](https://github.com/YIKUAIBANZI/memory-system-ops-skill/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/YIKUAIBANZI/memory-system-ops-skill?style=social)](https://github.com/YIKUAIBANZI/memory-system-ops-skill/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

A practical AgentSkill for operating a **fast + accurate + verifiable** personal memory system.

> 如果这个项目对你有帮助，欢迎点个 ⭐ Star。

---

## Who this is for

This project is useful if you are building assistants with:
- **OpenClaw**
- **Claude / GPT style agents**
- **Cursor + agent workflows**

Use it when your assistant suffers from:
- “you forgot” loops,
- messy profile/memory/task files,
- low-confidence historical recall,
- or task boards that never get cleaned.

---

## What this skill solves

Typical long-running assistant problems:
1. Facts are written, but hard to retrieve quickly.
2. Rules and profile information are mixed together.
3. Task boards grow forever and lose signal.
4. Interrupted tasks are hard to resume.
5. Memory quality degrades after many context resets.

`memory-system-ops` addresses these with a layered model, strict write-back structure, retrieval fallback order, and task archive discipline.

### Extra highlight: high-frequency vocabulary organization & lookup

This skill also works well for language-learning workflows (e.g., CET-6 / Kaoyan):
- keep a structured vocabulary project log in `memory/YYYY-MM-DD.md`,
- track recurring terms with `memory/keyword-frequency.md`,
- preserve active study actions in `TASKS.md` (`在做 / 就绪 / 中断`),
- quickly answer “这个词之前看过吗/出现过几次？” using the retrieval fallback path.

This turns ad-hoc word collection into a searchable, verifiable study memory system.

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

## What is in `SKILL.md`?

`SKILL.md` contains the executable operating policy.

Minimal structure:

```yaml
---
name: memory-system-ops
description: Operate a fast and reliable personal memory system...
---
```

Core instructions include:
- 3-layer memory model (`USER.md` / daily memory / `TASKS.md`)
- strict daily write-back fields (`Decision/Why/Impact/Next/Verify`)
- retrieval fallback (`L1 -> L2 -> L3`)
- task board rules for `在做/就绪/中断`
- keyword tracking update after each daily write-back

If you want full details, read [`SKILL.md`](./SKILL.md).

---

## Real usage snippets

### 1) Daily memory snippet

```markdown
- Decision: 将书单推送时间改为 13:00。
- Why: 用户明确要求中午接收，晚间打扰较强。
- Impact: 日程和触达体验更稳定。
- Next: 连续观察 7 天打开率。
- Verify: 3/6-3/12 推送日志显示按时执行。
```

### 2) TASKS board snippet

```markdown
| 状态 | 任务ID | 任务 | 说明 | 下一步 | Blocked by | Resume step |
|---|---|---|---|---|---|---|
| 在做 | T-250307-01 | 词汇资料整理 | 汇总六级+考研词汇 | 完成去重并导出CSV | - | - |
| 中断 | T-250307-02 | 远端部署 | 需要服务器权限 | 等待SSH授权 | 缺少root权限 | 拿到密钥后执行deploy.sh |
```

More templates:
- [`references/daily-template.md`](./references/daily-template.md)
- [`references/tasks-template.md`](./references/tasks-template.md)
- [`references/workflow.md`](./references/workflow.md)

---

## Repository structure

```text
memory-system-ops/
├── SKILL.md
├── README.md
├── LICENSE
└── references/
    ├── workflow.md
    ├── daily-template.md
    └── tasks-template.md
```

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

## Distribution tips (early-stage projects)

If you want early traction, publish short case studies in:
- Claude / OpenAI related communities
- Reddit (`r/AIAssistants`, `r/LocalLLaMA`, etc.)
- V2EX / X threads

Useful post format:
1. What concrete failure it solves (“you forgot”)
2. Before/after behavior
3. 1-minute setup steps
4. Link to examples/templates

---

## License

This project is licensed under the **MIT License**. See [`LICENSE`](./LICENSE).
