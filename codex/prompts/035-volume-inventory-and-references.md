# 035 — Volume inventory and references

- Phase: 04 Management
- Recommended reasoning: **medium**
- Depends on: **034**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 035`; resolve unfinished prerequisites if it refuses.

## Goal

Make storage relationships visible without reading volume data.

## Required implementation

1. Implement volumes ls/inspect with driver, scope, safe options and current container mount references.
2. Treat no observed references as a snapshot fact rather than proof deletion is safe.
3. Handle remote drivers, missing mountpoint and disappearing container references gracefully.
4. Keep volume content browsing, deletion and prune outside this release; preserve permission and output conventions.

## Acceptance and verification

- Test named/anonymous/external-driver volumes and absent optional fields.
- Verify volume commands never execute a remote filesystem read of volume contents.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/035.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 035 --evidence codex/tracking/evidence/035.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 035 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
