# 013 — Keys host verification and unattended jumps

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **012**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 013`; resolve unfinished prerequisites if it refuses.

## Goal

Reuse existing SSH credentials with clear setup failures.

## Required implementation

1. Use IdentityFile, SSH agent and known_hosts through native SSH with strict host-key checking; background probes must not request passwords.
2. Support a documented terminal bootstrap for missing host trust with independent fingerprint verification and key loading in the OS agent.
3. Test jump-host and destination credential handling separately; destination CLI options are not assumed to configure a ProxyJump child.
4. Do not persist passphrases, enable agent forwarding or edit trust files; place credential diagnostics on stderr and keep stdout machine-readable.

## Acceptance and verification

- Test unknown/changed host keys and unavailable encrypted keys on a disposable lab.
- Verify missing jump credentials fail or time out promptly without hidden input prompts.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/013.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 013 --evidence codex/tracking/evidence/013.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 013 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
