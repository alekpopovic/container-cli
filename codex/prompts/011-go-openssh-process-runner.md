# 011 — Go OpenSSH process runner

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **010**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 011`; resolve unfinished prerequisites if it refuses.

## Goal

Implement bounded subprocess execution and reliable cleanup.

## Required implementation

1. Use os/exec with explicit argument slices and context deadlines; read stdout/stderr concurrently through bounded readers.
2. Own and reap every child. CommandContext alone may not clean descendants; implement a Unix process-group cancellation strategy for noninteractive operations and finite pipe-drain waits.
3. Separate snapshot deadlines, connection deadlines and long-lived stream cancellation; never impose the default snapshot timeout on log follow.
4. Do not invoke a local shell; provide structured exit/error metadata to higher layers without exposing raw secrets.

## Acceptance and verification

- Use inert helper processes to test large stderr, held-open descendant pipes, cancellation and nonzero exit.
- Verify no goroutine/child leak remains after timeout or consumer cancellation.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/011.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 011 --evidence codex/tracking/evidence/011.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 011 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
