# 023 — Bounded log snapshots

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **022**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 023`; resolve unfinished prerequisites if it refuses.

## Goal

Retrieve recent logs with correct output and stream attribution limits.

## Required implementation

1. Implement containers logs with a bounded positive --tail, timestamps and validated --since/--until values.
2. Human output is safe text by default, JSON is a bounded records envelope and --output raw is an explicit byte passthrough mode.
3. Application stderr and SSH/Docker diagnostics share the SSH stderr channel; record channel provenance honestly instead of claiming definitive application-origin attribution.
4. Use process status for final failure, handle unavailable logging drivers and invalid UTF-8 and disclose truncation.

## Acceptance and verification

- Test application stderr, daemon errors, invalid UTF-8 and oversized lines.
- Verify finite JSON produces no partial stdout if the operation fails.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/023.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 023 --evidence codex/tracking/evidence/023.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 023 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
