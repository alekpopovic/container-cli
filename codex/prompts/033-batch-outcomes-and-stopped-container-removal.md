# 033 — Batch outcomes and stopped-container removal

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **032**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 033`; resolve unfinished prerequisites if it refuses.

## Goal

Handle small explicit batches with honest partial results.

## Required implementation

1. Bound target counts and action concurrency; preserve per-target outcomes and show partial failures without rolling back successful remote actions automatically.
2. Implement containers rm only for explicitly selected currently stopped containers; support neither force nor volume-removal flags.
3. Recheck state/identity before removal and report a race safely if a container restarts.
4. Return exit 7 for partial batch failures and 8 when any dispatched outcome is unknown; include all known per-target results.

## Acceptance and verification

- Test success/failure/unknown combinations and cancellation of queued targets.
- Verify running-container removal is refused and volumes are never deleted by this command.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/033.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 033 --evidence codex/tracking/evidence/033.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 033 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
