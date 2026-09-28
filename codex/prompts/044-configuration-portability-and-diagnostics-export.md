# 044 — Configuration portability and diagnostics export

- Phase: 05 Exec and automation
- Recommended reasoning: **medium**
- Depends on: **043**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 044`; resolve unfinished prerequisites if it refuses.

## Goal

Help users move non-secret preferences between Linux and macOS.

## Required implementation

1. Implement config path/show and sanitized export/import with schema validation, previews and explicit target file selection.
2. Separate application config from SSH config; importing metadata never imports keys, trust files, scripts or automatic write privileges.
3. Extend doctor with redacted support output, platform/tool versions and error stages; do not include raw logs, exec input or environment secrets.
4. Lock writes and preserve backups for concurrent commands or schema migration failures.

## Acceptance and verification

- Test paths with spaces/non-ASCII characters, missing home/config dirs and corrupt imports.
- Scan diagnostic exports for seeded secrets and verify import does not alter SSH files.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/044.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 044 --evidence codex/tracking/evidence/044.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 044 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
