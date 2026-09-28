# 031 — Mutation planning dry-run and activity records

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **030**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 031`; resolve unfinished prerequisites if it refuses.

## Goal

Plan remote changes explicitly without replaying uncertain actions.

## Required implementation

1. Implement invocation-scoped mutation plans containing host/daemon identity, operation and resolved target IDs, enforcing --host plus --write.
2. Add --dry-run to mutating commands: perform necessary reads and output the plan, dispatch no mutation, still enforce explicit target/write intent.
3. Require /dev/tty confirmation or --yes for execution; revalidate target identity and serialize conflicting operations.
4. Record bounded sanitized local activity if enabled, classifying transport loss after dispatch as unknown outcome; this is not a tamper-proof audit trail.

## Acceptance and verification

- Test plan/execute target changes, non-TTY confirmation and dry-run with a spy transport.
- Verify dropped responses are never interpreted as permission to automatically retry.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/031.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 031 --evidence codex/tracking/evidence/031.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 031 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
