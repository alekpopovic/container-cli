# 030 — Read-only CLI MVP checkpoint

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **029**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 030`; resolve unfinished prerequisites if it refuses.

## Goal

Deliver a usable command-line Docker inspector.

## Required implementation

1. Integrate hosts, list/inspect/detail commands, logs, stats/watch, events and Compose grouping into consistent help and examples.
2. Run direct and ProxyJump workflows against disposable infrastructure using both human and structured output.
3. Review stdout/stderr separation, empty results, bounds, signal cleanup and explicit target identity.
4. Write docs/checkpoints/030-read-only.md with real commands, statuses and remaining native-platform checks.

## Acceptance and verification

- Demonstrate list JSON consumed by jq or a JSON parser, live logs cancellation and bounded stats watch.
- Keep this checkpoint blocked if only fixture output is available for the integrated remote workflow.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/030.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 030 --evidence codex/tracking/evidence/030.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 030 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
