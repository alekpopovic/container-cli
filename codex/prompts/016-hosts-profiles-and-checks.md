# 016 — Hosts profiles and checks

- Phase: 02 Native SSH
- Recommended reasoning: **medium**
- Depends on: **015**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 016`; resolve unfinished prerequisites if it refuses.

## Goal

Connect saved metadata and SSH probes to usable commands.

## Required implementation

1. Implement hosts add/show/list/remove/use and hosts check, with explicit local versus remote behavior in help.
2. Use SSH aliases as profile keys, separate display labels/groups and allow reads through a configured default host.
3. Keep hosts list local unless an explicit check action is requested; removing a profile must not alter ~/.ssh/config or remote resources.
4. Display effective destination and daemon identity through the documented output encoders; sanitize diagnostic fields.

## Acceptance and verification

- Exercise add/use/list/remove with two identical display labels and distinct aliases.
- Confirm write operations still demand an explicit --host despite a saved default.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/016.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 016 --evidence codex/tracking/evidence/016.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 016 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
