# 049 — Fuzzing parsers and argument encoding

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **048**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 049`; resolve unfinished prerequisites if it refuses.

## Goal

Explore hostile input at the narrowest high-risk boundaries.

## Required implementation

1. Add focused Go fuzz targets for JSON-line decoding, bounded log records, alias validation and remote argument quoting.
2. Seed realistic examples including quotes, control characters, invalid UTF-8 and very long fields.
3. Assert bounded work/no panic, semantic argument preservation where applicable, and no accidental mutation or subprocess during pure parsing.
4. Use finite local fuzz budgets and preserve small reproductions of any discovered defects.

## Acceptance and verification

- Run each fuzz target for a documented bounded duration on the available platform.
- Record coverage limits and fix any reproducible crash before proceeding.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/049.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 049 --evidence codex/tracking/evidence/049.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 049 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
