# 050 — Disposable SSH and Docker integration lab

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **049**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 050`; resolve unfinished prerequisites if it refuses.

## Goal

Provide reproducible live transport tests without production access.

## Required implementation

1. Create an opt-in client/bastion/private-target lab with ephemeral keys, strict known_hosts and a dedicated disposable Docker daemon.
2. Keep the private target inaccessible directly from the client and test direct access using a separate intended alias.
3. Cover inventory, logs, stats, mutations, Compose, exec, host-key changes, rootless access where available and reconnect/cleanup.
4. Avoid mounting the users production Docker socket; any privileged Docker-in-Docker setup belongs in a dedicated disposable test VM with explicit cleanup.

## Acceptance and verification

- Run the suite and show the ProxyJump route plus failure isolation.
- Verify key/process/container cleanup and document exact OS/OpenSSH/Docker versions.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/050.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 050 --evidence codex/tracking/evidence/050.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 050 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
