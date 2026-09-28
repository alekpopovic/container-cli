# 022 — Focused container detail subcommands

- Phase: 03 Read-only MVP
- Recommended reasoning: **medium**
- Depends on: **021**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 022`; resolve unfinished prerequisites if it refuses.

## Goal

Offer concise health ports mounts and environment views.

## Required implementation

1. Implement containers health, ports, mounts and env from the shared inspect model rather than new shell parsing.
2. Differentiate running state from health configuration; distinguish exposed ports from host bindings including IPv6 and multiple bindings.
3. Keep env values masked unless --show-secrets is explicit; escape terminal control characters in human tables.
4. Use the same finite JSON envelope with command-appropriate items and clear empty/not-configured states.

## Acceptance and verification

- Check field consistency across inspect and detail commands.
- Exercise long paths, IPv6 ports, missing health and control characters without terminal escapes.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/022.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 022 --evidence codex/tracking/evidence/022.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 022 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
