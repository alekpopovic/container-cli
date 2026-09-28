# 007 — Execution interfaces and synthetic fixtures

- Phase: 01 CLI foundation
- Recommended reasoning: **medium**
- Depends on: **006**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 007`; resolve unfinished prerequisites if it refuses.

## Goal

Enable offline development through small testable Go interfaces.

## Required implementation

1. Define minimal interfaces for process execution, config storage, clock/limits, input confirmation and output rendering; keep command logic thin.
2. Create synthetic Docker inventory, inspect, logs, events and stats fixtures plus SSH failures and malformed/oversized output.
3. Label all fixture demonstrations explicitly and never fall back to fixture data after a failed real connection.
4. Add focused Go tests and temporary filesystem helpers; avoid package-level mutable command instances and init-time network calls.

## Acceptance and verification

- Run command construction repeatedly in one process and verify flags/state do not leak.
- Check offline tests spawn no SSH and real command errors never become demo success.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/007.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 007 --evidence codex/tracking/evidence/007.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 007 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
