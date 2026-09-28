# 017 — Structured SSH error classification

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **016**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 017`; resolve unfinished prerequisites if it refuses.

## Goal

Provide useful error codes without pretending every SSH failure is perfectly classifiable.

## Required implementation

1. Map configuration, timeout, transport/authentication, permission, remote command and unknown-outcome failures to the CLI error contract.
2. Keep bounded sanitized stderr excerpts with stage information; use conservative fallback classification for locale-dependent OpenSSH messages.
3. Return JSON diagnostics on stderr in structured output modes and concise text otherwise, with no duplicated Cobra error/usage blocks.
4. Implement a hosts check walkthrough that reports stages and remaining setup without dumping keys or full config.

## Acceptance and verification

- Test non-English/unknown stderr, SSH status 255, remote Docker nonzero and malformed JSON independently.
- Ensure all paths return nonzero and stdout remains empty for a failed finite request.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/017.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 017 --evidence codex/tracking/evidence/017.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 017 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
