# 012 — Remote command validation and quoting

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **011**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 012`; resolve unfinished prerequisites if it refuses.

## Goal

Encode remote argv safely across the OpenSSH shell boundary.

## Required implementation

1. Create typed Docker operation builders plus a central POSIX single-quote encoder for every remote argument.
2. Document that local os/exec argument slices do not remove the remote shell. Target POSIX-compatible remote login shells and report unsupported setups.
3. Validate IDs, numeric bounds, fixed flags and optional absolute remote paths before quoting; templates such as {{json .}} remain literal.
4. For later exec passthrough, accept explicit user argv after -- and quote each token; never substitute remote labels or config metadata as shell code.

## Acceptance and verification

- Test empty strings, quotes, semicolons, backticks, dollar substitution, newlines and option-like values against an inert shell harness.
- Confirm one user argv token stays one remote token and cannot append a host-side command.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/012.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 012 --evidence codex/tracking/evidence/012.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 012 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
