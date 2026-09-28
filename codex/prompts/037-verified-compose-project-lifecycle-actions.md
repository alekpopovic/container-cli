# 037 — Verified Compose project lifecycle actions

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **036**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 037`; resolve unfinished prerequisites if it refuses.

## Goal

Operate existing Compose services using explicit remote project configuration.

## Required implementation

1. Add compose start/stop/restart PROJECT with required verified --project-dir and ordered --file values plus optional explicit service selection.
2. Validate and quote remote paths, set project identity deliberately and preserve daemon context/privilege mode.
3. Apply the same --host/--write/confirmation/dry-run policy and require readable remote files; do not execute paths obtained only from labels.
4. Limit operations to existing services; no compose up/down/build/pull, resolved-secret export or volume removal.

## Acceptance and verification

- Test multiple files, paths with spaces/apostrophes, missing env dependencies and wrong project names.
- Run one disposable project action and verify unrelated projects remain untouched.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/037.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 037 --evidence codex/tracking/evidence/037.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 037 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
