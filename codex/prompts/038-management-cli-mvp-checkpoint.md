# 038 — Management CLI MVP checkpoint

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **037**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 038`; resolve unfinished prerequisites if it refuses.

## Goal

Verify readable outcomes and policy across the management command set.

## Required implementation

1. Demonstrate read-only defaults, dry-run, confirmation, --yes and lifecycle/Compose actions against a disposable jump-host target.
2. Review resource list/inspect commands and action help for consistent target and output semantics.
3. Exercise stale selection, partial failure, changed daemon identity and unknown mutation outcome.
4. Write docs/checkpoints/038-management.md with actual results and unresolved compatibility gates.

## Acceptance and verification

- Call services directly to confirm mutations cannot bypass policy through another command path.
- Validate action JSON and exit codes in shell scripts; block missing live functionality rather than declaring fixture success.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/038.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 038 --evidence codex/tracking/evidence/038.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 038 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
