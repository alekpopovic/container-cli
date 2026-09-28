# 041 — Signals process groups and broken pipes

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **040**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 041`; resolve unfinished prerequisites if it refuses.

## Goal

Make termination reliable for terminals and automation.

## Required implementation

1. Integrate SIGINT/SIGTERM handling with contexts and bounded owned-process shutdown for snapshot/stream commands.
2. Keep interactive exec foreground/terminal handling separate; do not put it into a detached process session that loses its controlling terminal.
3. Handle writer errors and EPIPE deliberately, documenting exit 141 for a closed data pipe after cleanup and 130/143 for wrapper cancellation.
4. Ensure cleanup precedes final exit and raw exec preserves child status where available without duplicate signal delivery.

## Acceptance and verification

- Run subprocess tests for SIGINT/SIGTERM and piping large output to head.
- Verify child/goroutine counts return to baseline and no cleanup is skipped by an early os.Exit.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/041.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 041 --evidence codex/tracking/evidence/041.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 041 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
