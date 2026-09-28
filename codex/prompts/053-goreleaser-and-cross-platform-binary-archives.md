# 053 — GoReleaser and cross-platform binary archives

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **052**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 053`; resolve unfinished prerequisites if it refuses.

## Goal

Produce reproducible CLI artifacts for the four target OS/architecture pairs.

## Required implementation

1. Configure the current compatible GoReleaser schema for linux/darwin and amd64/arm64, starting with CGO_ENABLED=0 if dependencies permit.
2. Embed version/commit metadata and use release-tag/source-date inputs for reproducibility; do not require a GUI packaging toolchain.
3. Create tar.gz archives containing the binary, license if chosen, README, completion scripts and checksums.
4. Run snapshot builds locally without publishing and document any dependencies that prevent a pure-Go build.

## Acceptance and verification

- Build all configured targets and inspect archive paths/permissions and binary metadata.
- Record cross-build success separately from native runtime verification.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/053.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 053 --evidence codex/tracking/evidence/053.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 053 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
