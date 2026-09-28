# 009 — SSH config candidate discovery

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **008**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 009`; resolve unfinished prerequisites if it refuses.

## Goal

Discover concrete aliases without reimplementing OpenSSH.

## Required implementation

1. Implement hosts discover using ~/.ssh/config or a chosen config; follow Include paths with bounded size/depth and cycle detection.
2. Extract literal Host aliases including multiple names per line while excluding wildcards/negation from concrete candidates.
3. Support manually adding an alias when configuration is dynamic or cannot be enumerated; no automatic connection scan.
4. Never modify SSH config and do not evaluate Match exec/ProxyCommand during discovery; report partial discovery clearly.

## Acceptance and verification

- Test quoted include paths, includes with spaces, comments, multiple aliases, glob patterns and cycles.
- Verify discovery invokes no subprocess and never treats a wildcard as a concrete host.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/009.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 009 --evidence codex/tracking/evidence/009.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 009 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
