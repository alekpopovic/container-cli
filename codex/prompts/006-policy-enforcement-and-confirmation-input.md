# 006 — Policy enforcement and confirmation input

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **005**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 006`; resolve unfinished prerequisites if it refuses.

## Goal

Require deliberate target selection for remote changes.

## Required implementation

1. Implement service-level read versus mutation/exec policy; changing resources requires explicit --host and --write in this invocation.
2. Add --yes as confirmation bypass only after policy passes. For interactive confirmation read from /dev/tty, not piped stdin reserved for exec input.
3. Noninteractive calls lacking --yes fail promptly with the policy exit code, before dispatching a mutation.
4. Bind the operation plan to the resolved daemon identity and exact IDs; print a concise confirmation on stderr and revalidate immediately before execution.

## Acceptance and verification

- Call service operations without Cobra to prove the policy is not only a UI check.
- Test --yes without --write, implicit host on a write, and piped stdin preservation during confirmation.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/006.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 006 --evidence codex/tracking/evidence/006.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 006 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
