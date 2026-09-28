# 008 — Local doctor and deterministic startup

- Phase: 01 CLI foundation
- Recommended reasoning: **medium**
- Depends on: **007**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 008`; resolve unfinished prerequisites if it refuses.

## Goal

Diagnose local prerequisites without requiring Docker locally.

## Required implementation

1. Implement doctor local for OS/architecture, executable location, version, config permissions and agent-socket availability.
2. Prefer a validated absolute native SSH path, normally /usr/bin/ssh, with an explicit override; do not execute a shell to discover PATH.
3. Resolve home/config paths with Go OS APIs and support spaces/non-ASCII paths; keep startup free of unrequested host probes.
4. Document that the installed app requires its binary plus native OpenSSH; Python is only the optional development prompt tracker.

## Acceptance and verification

- Test missing/non-executable SSH override, minimal PATH and missing agent socket.
- Confirm version, help and shell-completion script generation remain available without SSH.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/008.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 008 --evidence codex/tracking/evidence/008.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 008 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
