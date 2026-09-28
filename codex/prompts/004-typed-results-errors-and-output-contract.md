# 004 — Typed results errors and output contract

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **003**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 004`; resolve unfinished prerequisites if it refuses.

## Goal

Make the CLI dependable for jq and shell scripts.

## Required implementation

1. Define Go domain models and v1 JSON envelopes exactly as CLI_CONTRACT.md specifies; model typed errors with stable application exit codes.
2. Implement table/text output with color disabled off-TTY and versioned JSON/JSONL encoders using injected io.Writers.
3. Finite --output json must buffer a bounded result until success; send only one data document to stdout and diagnostics to stderr.
4. Limit raw output to explicitly supported passthrough/log commands. Streaming JSONL uses one complete object per line and reports later errors separately.

## Acceptance and verification

- Parse captured stdout as JSON and JSONL while deliberately emitting diagnostics.
- Test empty collections as [] and failures without partial finite JSON; verify exit codes in subprocess-level checks.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/004.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 004 --evidence codex/tracking/evidence/004.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 004 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
