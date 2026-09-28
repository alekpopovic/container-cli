# 040 — Interactive exec with native terminal behavior

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **039**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 040`; resolve unfinished prerequisites if it refuses.

## Goal

Support a real shell through the users existing terminal.

## Required implementation

1. Add -t and -i behavior with explicit TTY detection; -t requires an actual local terminal and -it is the normal interactive shell form.
2. Attach local terminal file descriptors directly to native OpenSSH, request a remote PTY and run Docker exec with the matching flags.
3. Prefer native SSH to own local terminal mode/resize rather than embedding a full terminal emulator; define signal ownership to avoid double forwarding.
4. Restore any state changed by DockJump on every ordinary error/cancel path; report missing shells and exited containers cleanly.

## Acceptance and verification

- Use a PTY-based integration harness to check Ctrl-C, resize, shell exit and abrupt SSH failure.
- Verify terminal settings are restored afterward and structured read commands never allocate a PTY.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/040.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 040 --evidence codex/tracking/evidence/040.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 040 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
