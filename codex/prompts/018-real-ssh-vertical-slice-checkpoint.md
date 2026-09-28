# 018 — Real SSH vertical slice checkpoint

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **017**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 018`; resolve unfinished prerequisites if it refuses.

## Goal

Prove a direct and ProxyJump Docker read before expanding the CLI.

## Required implementation

1. Use disposable client/bastion/private-target resources with temporary keys and strict dedicated known_hosts.
2. Run hosts check and a bounded remote Docker JSON probe with no Docker or jq on the client.
3. Document topology, versions, commands, actual results and process cleanup in docs/checkpoints/018-ssh.md.
4. Record missing native platforms separately; mock transport results cannot satisfy this live SSH gate.

## Acceptance and verification

- Demonstrate the private target is accessed through the jump host and the CLI cleans its owned sessions.
- If a real disposable SSH target is unavailable, retain the checkpoint as blocked with the exact next prerequisite.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/018.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 018 --evidence codex/tracking/evidence/018.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 018 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
