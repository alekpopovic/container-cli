# 055 — macOS native binary acceptance

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **054**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 055`; resolve unfinished prerequisites if it refuses.

## Goal

Verify Apple Silicon and Intel terminal workflows.

## Required implementation

1. Run the packaged darwin binaries natively on documented supported macOS versions and record architecture/tool versions.
2. Test /usr/bin/ssh, agent/encrypted-key access, path handling, strict host trust and jump connectivity.
3. Exercise logs/watch cancellation, exec resize/Ctrl-C and completion in zsh plus bash where supported.
4. Keep unsigned local execution and public signing/notarization status separate; no Gatekeeper bypass is the prescribed installation flow.

## Acceptance and verification

- Record real results for arm64 and amd64; a cross-compiled binary alone is not acceptance.
- If a required Mac/runner is unavailable, retain the prompt as blocked rather than substituting Linux results.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/055.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 055 --evidence codex/tracking/evidence/055.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 055 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
