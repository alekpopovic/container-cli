# 045 — Performance bounds and streaming stress

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **044**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 045`; resolve unfinished prerequisites if it refuses.

## Goal

Measure the CLI under large inventories and output pressure.

## Required implementation

1. Benchmark decoding/rendering 1000 containers, large inspect records and high-rate logs/events on a documented machine.
2. Set explicit byte/record/queue/concurrency limits and test long lines beyond bufio.Scanner defaults; do not rely on default token sizes silently.
3. Prefer incremental JSONL streaming and bounded finite snapshots; avoid retaining full history or one subprocess per row.
4. Record actual wall time, peak memory and process/goroutine behavior without inventing cross-platform benchmark numbers.

## Acceptance and verification

- Confirm memory reaches a bound under a slow writer and cancellation closes owned children.
- Measure finite output and record performance limits in docs/benchmarks.md.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/045.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 045 --evidence codex/tracking/evidence/045.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 045 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
