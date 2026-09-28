# 014 — Invocation-scoped connection reuse

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **013**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 014`; resolve unfinished prerequisites if it refuses.

## Goal

Reuse connections during one command without adding a resident daemon.

## Required implementation

1. Create an app-owned control master/socket when one invocation runs several reads, watch loops or a group query.
2. Keep private socket paths short and mode-restricted on Linux/macOS; use unique ownership per invocation and bounded persistence.
3. For single operations permit a simpler path; explicitly avoid taking ownership of a user control master from their config.
4. Clean up only owned processes/sockets on completion and provide a bounded fallback if multiplexing is unavailable.

## Acceptance and verification

- Test simultaneous invocations, stale sockets and long home paths.
- Confirm one command exiting cannot close another invocation or the users existing SSH master.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/014.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 014 --evidence codex/tracking/evidence/014.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 014 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
