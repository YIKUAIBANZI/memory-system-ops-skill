# Workflow

1. Read current state:
   - `USER.md`
   - `TASKS.md`
   - `memory/YYYY-MM-DD.md`
   - `agent-memory/common-info.md`
2. Apply write-back standard for new decisions.
3. If the query is keyword-driven, run keyword-first lookup:
   - check `memory/keyword-frequency.md`
   - locate matching entries in `memory/*.md`
   - provide file-based evidence.
4. If history query confidence is low, run retrieval fallback (L1->L2->L3).
5. Update task board (including Blocked by / Resume step if interrupted).
6. Archive completed tasks into `TASKS_ARCHIVE/YYYY-MM.md`.
7. Update keyword frequency immediately after daily memory write.
8. For major rule changes, record change reason in daily memory with verification evidence.
