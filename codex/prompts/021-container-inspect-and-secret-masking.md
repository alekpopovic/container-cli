# 021 — Container inspect and secret masking

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **020**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 021`; resolve unfinished prerequisites if it refuses.

## Goal

Expose accurate details without revealing secrets by default.

## Required implementation

1. Implement containers inspect TARGET through inspect JSON arrays and normalize mounts, health, exit data, network addresses and restart configuration.
2. Mask all environment values and likely-sensitive label values before output encoding; keep inspect result copies out of app diagnostics.
3. Support explicit --show-secrets only for the requested result, with a warning on stderr and no persistence.
4. Handle container disappearance, absent health, null fields and large data within documented bounds.

## Acceptance and verification

- Test stopped, healthcheck-free, rootless and multi-network fixtures.
- Scan default stdout/stderr and local files for seeded environment/label secrets.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/021.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 021 --evidence codex/tracking/evidence/021.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 021 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
