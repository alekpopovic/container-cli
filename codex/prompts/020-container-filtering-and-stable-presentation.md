# 020 — Container filtering and stable presentation

- Phase: 03 Read-only MVP
- Recommended reasoning: **medium**
- Depends on: **019**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 020`; resolve unfinished prerequisites if it refuses.

## Goal

Make inventory useful in terminals and shell pipelines.

## Required implementation

1. Add --all, supported --filter values, --sort and --quiet with precisely defined semantics; default listing shows running containers and --all includes stopped.
2. Render safe human tables using a stable column order and no ANSI in pipes; --quiet emits full IDs one per line and conflicts with structured output flags.
3. JSON uses the documented items envelope and stable types, independent of terminal width.
4. Resolve target names for later detail operations by exact match or unique ID prefix on the selected daemon; ambiguity fails with a useful error.

## Acceptance and verification

- Test duplicate-looking names across hosts, ambiguous prefixes and malformed filters.
- Pipe JSON through a parser and verify human formatting never modifies machine fields.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/020.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 020 --evidence codex/tracking/evidence/020.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 020 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
