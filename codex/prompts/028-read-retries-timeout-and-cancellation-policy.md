# 028 — Read retries timeout and cancellation policy

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **027**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 028`; resolve unfinished prerequisites if it refuses.

## Goal

Make slow connections predictable across all read commands.

## Required implementation

1. Centralize bounded retry/backoff for idempotent read setup with stage-specific deadlines and jitter.
2. Do not retry a finite request after writing a successful/partial result to stdout unless its output protocol explicitly represents retries.
3. Treat log/event reconnection as opt-in and report gaps; keep stream lifetime separate from snapshot timeout.
4. Add elapsed timings and actionable errors on stderr without changing deterministic result schemas.

## Acceptance and verification

- Simulate network loss before and after output and assert the contract for each mode.
- Check that no mutation or exec command can use automatic retry code paths.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/028.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 028 --evidence codex/tracking/evidence/028.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 028 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
