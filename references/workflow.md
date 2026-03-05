# Workflow

1. Read current state:
   - `USER.md`
   - `TASKS.md`
   - `memory/YYYY-MM-DD.md`
   - `agent-memory/common-info.md`
2. Apply write-back standard for new decisions.
3. If history query confidence is low, run retrieval fallback (L1->L2->L3).
4. Update task board (including Blocked by / Resume step if interrupted).
5. Archive completed tasks into `TASKS_ARCHIVE/YYYY-MM.md`.
6. Update keyword frequency immediately after daily memory write.
7. For major rule changes, record change reason in daily memory with verification evidence.
