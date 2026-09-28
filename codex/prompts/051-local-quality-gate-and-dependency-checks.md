# 051 — Local quality gate and dependency checks

- Phase: 06 Quality and delivery
- Recommended reasoning: **medium**
- Depends on: **050**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 051`; resolve unfinished prerequisites if it refuses.

## Goal

Give contributors one reproducible verification path.

## Required implementation

1. Create gofmt-check, go vet, unit tests, applicable race tests, build and optional integration/fuzz targets in a simple Makefile.
2. Run govulncheck with a pinned compatible tool version when available and distinguish unavailable checks from passing results.
3. Use go.mod/go.sum and a pinned Go toolchain; record CGO settings and OS-specific files explicitly.
4. Do not download/update toolchains silently during an offline verification claim; record network requirements and missing dependencies.

## Acceptance and verification

- Run the standard gate from a clean dependency state where possible.
- Temporarily fail one required check to verify the aggregate command returns nonzero, then restore the code.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/051.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 051 --evidence codex/tracking/evidence/051.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 051 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
