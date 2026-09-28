# 019 — Container inventory adapter

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **018**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 019`; resolve unfinished prerequisites if it refuses.

## Goal

Normalize Docker JSON lines into a stable container model.

## Required implementation

1. Implement containers ls using docker ps -a --no-trunc with a JSON template through the central builder.
2. Preserve full IDs, names, image, state, display status and ports while tolerating absent optional/new fields.
3. Distinguish successful zero rows from malformed output, banners, nonzero exit or a truncated response.
4. Bound total bytes, records and per-record decoding; do not silently discard arbitrary output lines.

## Acceptance and verification

- Test empty, running, exited, restarting and malformed fixtures with Unicode values.
- Compare normalized count/IDs with the disposable target CLI.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/019.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 019 --evidence codex/tracking/evidence/019.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 019 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
