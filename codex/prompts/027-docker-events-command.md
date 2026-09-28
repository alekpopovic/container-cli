# 027 — Docker events command

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **026**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 027`; resolve unfinished prerequisites if it refuses.

## Goal

Expose a bounded event stream with explicit gap semantics.

## Required implementation

1. Implement events with validated filters and --since/--until using Docker event JSON output; support safe text and JSONL.
2. Distinguish events from complete inventory and note daemon retention/reconnect gaps; use a fresh snapshot for reconciliation.
3. Enforce record/output bounds and reuse signal, writer-error and child cleanup handling.
4. Keep event command execution explicit, with no persistent background collector or unselected host subscriptions.

## Acceptance and verification

- Test duplicate/burst/missing-field events and a reconnect gap.
- Confirm a downstream pipe closing immediately does not leave an SSH process behind.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/027.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 027 --evidence codex/tracking/evidence/027.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 027 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
