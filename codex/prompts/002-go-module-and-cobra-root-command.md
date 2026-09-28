# 002 — Go module and Cobra root command

- Phase: 01 CLI foundation
- Recommended reasoning: **medium**
- Depends on: **001**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 002`; resolve unfinished prerequisites if it refuses.

## Goal

Create a minimal runnable CLI without remote side effects.

## Required implementation

1. Initialize one Go module in the existing root; use a clearly local module path if no repository URL is provided and document renaming later.
2. Create cmd/dockjump/main.go and internal/cli constructors that return fresh Cobra command trees with injected streams/dependencies.
3. Add version and root help; use RunE and a top-level run function whose cleanup finishes before main calls os.Exit.
4. Commit go.mod/go.sum and a small Makefile or equivalent documented build/test commands; avoid generating unrelated service scaffolding.

## Acceptance and verification

- Run go test ./... and go build ./cmd/dockjump.
- Run help/version without SSH installed in the test environment and confirm they still work.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/002.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 002 --evidence codex/tracking/evidence/002.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 002 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
