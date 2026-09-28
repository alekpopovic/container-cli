# 025 — Container statistics snapshots

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **024**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 025`; resolve unfinished prerequisites if it refuses.

## Goal

Provide normalized metrics suitable for inspection and scripts.

## Required implementation

1. Implement containers stats with --once default and bounded docker stats --no-stream --no-trunc JSON-template requests.
2. Normalize unit strings while retaining appropriate raw display values; CPU may exceed 100 and memory follows Docker CLI reporting semantics.
3. Represent missing/stopped samples as unavailable, not measured zero; label sample timestamps and daemon identity.
4. Restrict one inflight stats snapshot per host and avoid one subprocess per displayed row.

## Acceptance and verification

- Test KiB/MiB/GiB conversion, unknown values, CPU over 100 and container disappearance.
- Compare selected samples with native Docker CLI output on the lab target.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/025.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 025 --evidence codex/tracking/evidence/025.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 025 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
