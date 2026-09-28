# 048 — Command contract and regression suite

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **047**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 048`; resolve unfinished prerequisites if it refuses.

## Goal

Lock down important behavior through external observable tests.

## Required implementation

1. Build table-driven tests for argument validation, config precedence, writer separation, exit codes and output schemas.
2. Cover empty/malformed Docker responses, masking, partial results, interrupted writes and remote command quoting.
3. Use fresh Cobra instances with injected dependencies and subprocess tests where actual process exit/signals matter.
4. Avoid snapshots that merely repeat implementation; tests should fail on a realistic contract regression.

## Acceptance and verification

- Run go test ./... and a race-enabled test run where supported.
- Demonstrate one controlled regression is caught, restore it and record exact check results.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/048.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 048 --evidence codex/tracking/evidence/048.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 048 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
