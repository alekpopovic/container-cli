# 043 — Shell completion and generated reference docs

- Phase: 05 Exec and automation
- Recommended reasoning: **medium**
- Depends on: **042**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 043`; resolve unfinished prerequisites if it refuses.

## Goal

Provide useful completion without unexpected remote execution.

## Required implementation

1. Generate bash, zsh and fish completion scripts with Cobra and document user-installed locations.
2. Complete command/flag names and saved literal host aliases locally; dynamic container completion uses only explicitly cached local metadata if available.
3. Do not run ssh -G, connect to servers or read private keys during completion; allow local completion to fail quietly if config is absent.
4. Generate reference Markdown/man pages from the implemented command tree and keep examples aligned with actual flags.

## Acceptance and verification

- Smoke-test completion generation and shell syntax on available shells.
- Instrument completion to prove no transport call occurs and verify help generation is deterministic.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/043.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 043 --evidence codex/tracking/evidence/043.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 043 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
