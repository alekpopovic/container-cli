# 046 — Feature-complete CLI checkpoint

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **045**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 046`; resolve unfinished prerequisites if it refuses.

## Goal

Review the complete CLI before packaging.

## Required implementation

1. Run the end-to-end host discovery/check, read commands, lifecycle, Compose, exec, completion and scripting journey.
2. Remove misleading placeholder commands, stale help and inconsistent flag behavior.
3. Verify there is no resident service, GUI dependency or local Docker requirement in the built CLI.
4. Write docs/checkpoints/046-feature-complete.md with verified features and explicit remaining platform/build gates.

## Acceptance and verification

- Run both human and structured workflows on the current native OS against the disposable target.
- Block any missing core feature; distinguish upcoming native-platform acceptance from completed functionality.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/046.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 046 --evidence codex/tracking/evidence/046.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 046 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
