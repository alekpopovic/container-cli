# 024 — Log follow backpressure and cancellation

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **023**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 024`; resolve unfinished prerequisites if it refuses.

## Goal

Support long-lived log streams in both terminals and pipelines.

## Required implementation

1. Add containers logs --follow with safe text or JSONL records; reject one-document JSON for an unbounded stream.
2. Use bounded queues, finite line sizes and explicit dropped/truncated markers when the consumer cannot keep up.
3. Stop remote child activity on context cancellation, stdout write failure or a closed downstream pipe; do not retain an ever-growing log history.
4. If an optional read reconnect is enabled, mark gaps and best-effort timestamp resumption; never claim a lossless log transport.

## Acceptance and verification

- Run a high-rate synthetic stream into a slow/closed writer and verify bounded memory/child cleanup.
- Send SIGINT during live log follow and confirm controlled termination with a documented status.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/024.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 024 --evidence codex/tracking/evidence/024.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 024 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
