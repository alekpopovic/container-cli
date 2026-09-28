# 001 — Repository audit and CLI contract

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **none**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 001`; resolve unfinished prerequisites if it refuses.

## Goal

Establish the Go/Cobra implementation baseline for DockJump.

## Required implementation

1. Inspect existing code, Git state and local instructions without overwriting unrelated work. Record the proposed app name DockJump and binary dockjump as working names.
2. Write docs/decisions/0001-cli.md: one Go module, Cobra command tree, native OpenSSH, remote Linux Docker Engine, Linux/macOS clients and no resident daemon.
3. Select a supported stable Go version and compatible Cobra dependencies using official sources, pin the module/toolchain and record the development environment.
4. Adopt codex/docs/CLI_CONTRACT.md, architecture and sequential milestone gates; list required test infrastructure honestly.

## Acceptance and verification

- Record repository/tool versions and conflicts with existing instructions.
- Confirm every proposed command uses the documented stdout/stderr, host-selection and permission rules.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/001.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 001 --evidence codex/tracking/evidence/001.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 001 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
