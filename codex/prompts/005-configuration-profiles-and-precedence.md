# 005 — Configuration profiles and precedence

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **004**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 005`; resolve unfinished prerequisites if it refuses.

## Goal

Store non-secret host metadata and predictable defaults.

## Required implementation

1. Use schema-versioned JSON in os.UserConfigDir()/dockjump/config.json with atomic writes, owner-only permissions and cross-process write locking.
2. Resolve ordinary preferences as explicit flags, documented DOCKJUMP_* environment variables, config defaults, then built-in defaults.
3. Keep --write and --yes invocation-only capabilities; never grant them implicitly from environment or a profile.
4. Store aliases, groups, labels, optional remote Docker context/binary path and default host; never store keys, passwords, container environment values or transcripts.

## Acceptance and verification

- Test flags versus environment versus config precedence and simultaneous profile updates.
- Exercise corrupt config recovery and preserve the original file rather than overwriting it silently.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/005.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 005 --evidence codex/tracking/evidence/005.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 005 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
