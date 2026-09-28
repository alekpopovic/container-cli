# 026 — Watch mode for inventory and statistics

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **025**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 026`; resolve unfinished prerequisites if it refuses.

## Goal

Add lightweight terminal monitoring without a full-screen framework.

## Required implementation

1. Implement containers ls --watch and containers stats --watch with validated --interval and --count for bounded automation.
2. For TTY human output, update readable screens conservatively; for non-TTY emit successive labeled text snapshots or explicit JSONL, with no ANSI screen controls.
3. Use one invocation-owned SSH connection and one inflight refresh, bounded buffers and cancellation between samples.
4. Reject --output json for multi-snapshot modes, stop when count is reached and surface stale/failed samples instead of empty success.

## Acceptance and verification

- Use fake clocks to test interval/count, slow snapshots and cancellation.
- Verify redirected watch output is parseable JSONL when requested and contains no terminal escape sequences.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/026.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 026 --evidence codex/tracking/evidence/026.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 026 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
