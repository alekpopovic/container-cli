# 060 — Final handover and CLI release gate

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **059**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 060`; resolve unfinished prerequisites if it refuses.

## Goal

Hand over a complete, reproducible CLI project with truthful execution tracking.

## Required implementation

1. Write docs/FINAL_HANDOVER.md with build/test/install commands, command reference, architecture decisions and artifact hashes/locations.
2. Summarize actual native support, known limitations, public distribution readiness and optional future enhancements.
3. Validate prompt statuses/evidence and repository contents without deleting unrelated user work.
4. Complete only when agreed core/native gates are satisfied; otherwise record the exact blocker and next safe action instead of asserting release readiness.

## Acceptance and verification

- Run tracker validate and the final direct/ProxyJump CLI smoke scenario against the tested artifact.
- Ensure output/exit-code documentation matches the actual binary and no credentials/test secrets are included in archives.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/060.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 060 --evidence codex/tracking/evidence/060.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 060 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
