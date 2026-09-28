# 054 — Linux native binary acceptance

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **053**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 054`; resolve unfinished prerequisites if it refuses.

## Goal

Verify real operation of Linux artifacts.

## Required implementation

1. Install the generated Linux binary in a temporary user path on the documented baseline and run help/version/doctor without development tools.
2. Run direct and ProxyJump list/logs/stats, mutation and exec workflows with strict known_hosts.
3. Test signals, downstream closed pipes, completion and config paths; inspect child processes after exit.
4. Record amd64 and arm64 runtime results separately, including the exact runner/VM/physical environment; keep untested targets explicit.

## Acceptance and verification

- Execute the native smoke suite for every Linux architecture claimed verified.
- If a required native environment is missing, keep its acceptance blocked with a precise setup action.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/054.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 054 --evidence codex/tracking/evidence/054.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 054 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
