# 029 — Compose discovery and service grouping

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **028**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 029`; resolve unfinished prerequisites if it refuses.

## Goal

Group remote containers without requiring local Compose files.

## Required implementation

1. Implement compose ls using remote Compose JSON when available and label-derived grouping as a read fallback.
2. Implement compose ps PROJECT using selected-daemon containers and service labels; support projects with multiple service instances.
3. Do not treat reported ConfigFiles/working-directory labels as trusted executable configuration or local paths.
4. Distinguish plugin absence from no projects and keep project identity scoped to host/daemon.

## Acceptance and verification

- Test absent plugin, duplicate service names in different projects and deleted containers.
- Verify no local project files are required for read-only grouping.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/029.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 029 --evidence codex/tracking/evidence/029.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 029 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
