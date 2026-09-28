# 010 — Effective SSH resolution and trust boundary

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **009**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 010`; resolve unfinished prerequisites if it refuses.

## Goal

Use native OpenSSH for actual connection semantics.

## Required implementation

1. Resolve one explicitly selected alias with bounded ssh -G output, preserving the same executable and optional -F config for subsequent calls.
2. Validate alias syntax against a documented conservative grammar and reject leading dashes, whitespace and control characters.
3. Retain the original alias for connections and show effective host/user/port/jump data as diagnostics.
4. Treat user SSH config as trusted executable configuration: ssh -G can run Match exec. Never execute downloaded/unselected configs to populate completions.

## Acceptance and verification

- Compare diagnostics with native ssh -G on synthetic trusted configs with overlapping Host rules.
- Verify bad aliases fail before process creation and config paths are passed as arguments.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/010.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 010 --evidence codex/tracking/evidence/010.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 010 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
