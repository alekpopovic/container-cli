# 032 — Container start stop and restart commands

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **031**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 032`; resolve unfinished prerequisites if it refuses.

## Goal

Implement essential lifecycle actions with outcome verification.

## Required implementation

1. Add containers start/stop/restart for explicit target lists using current full IDs and fixed command builders.
2. Support bounded --stop-timeout where applicable and validate it before contacting the daemon.
3. Return action results through human tables or JSON and perform a read to verify observed post-action state without claiming application readiness.
4. After a lost response return the unknown-outcome contract and advise a read refresh; never repeat the action automatically.

## Acceptance and verification

- Run lifecycle actions on disposable containers and compare final state.
- Interrupt connectivity after submission and confirm one dispatch plus an unknown outcome.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/032.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 032 --evidence codex/tracking/evidence/032.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 032 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
