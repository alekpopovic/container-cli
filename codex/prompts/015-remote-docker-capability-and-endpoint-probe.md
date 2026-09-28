# 015 — Remote Docker capability and endpoint probe

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **014**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 015`; resolve unfinished prerequisites if it refuses.

## Goal

Identify the exact daemon that subsequent commands will affect.

## Required implementation

1. Probe remote client/server versions, daemon identity, Linux OS support, permissions and Compose plugin availability with bounded JSON output.
2. Use the remote users default Docker endpoint or an explicit named remote context consistently on every operation.
3. Show SSH destination separately from actual Docker endpoint, including rootless sockets or contexts that point elsewhere.
4. Support an explicit fixed sudo -n Docker mode for an already configured remote policy. Never install sudo rules, request passwords or change server access.

## Acceptance and verification

- Test missing Docker, socket denial, stopped daemon, rootless context, Compose absence and sudo needing a password.
- Verify selected context, binary and privilege mode propagate to every command builder.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/015.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 015 --evidence codex/tracking/evidence/015.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 015 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
