# 042 — Read-only host groups and fleet snapshots

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **041**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 042`; resolve unfinished prerequisites if it refuses.

## Goal

Query selected host groups with bounded concurrency.

## Required implementation

1. Add hosts group metadata and a mutually exclusive --group selector for finite supported read commands only.
2. Execute a bounded number of host reads, with identity on every row/item and stable final ordering for finite JSON.
3. Represent individual host errors and exit 7 for partial fleet results; do not substitute cached/empty data for a failed host.
4. Reject groups for mutations, exec and infinite streams in v1 to keep targeting clear.

## Acceptance and verification

- Test one successful and one denied host, shared container names and deterministic JSON order.
- Measure concurrency against configured bounds and verify no host outside the selected group is contacted.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/042.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 042 --evidence codex/tracking/evidence/042.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 042 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
