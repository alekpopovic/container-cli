# 058 — Serbian quick start and English runbook

- Phase: 06 Quality and delivery
- Recommended reasoning: **medium**
- Depends on: **057**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 058`; resolve unfinished prerequisites if it refuses.

## Goal

Document the implemented tool for terminal users and automation.

## Required implementation

1. Write Serbian install/SSH/ProxyJump quick start and English architecture/troubleshooting/runbook docs.
2. Include jq examples, stdout/stderr handling, shell pipefail guidance, completion, --write/--yes policy and explicit exec -- boundaries.
3. Explain Docker endpoint/context, rootless/configured sudo -n modes, Compose remote paths and unknown mutation outcome recovery.
4. Cross-check every flag and example against generated command help, with placeholders clearly labeled and no insecure SSH shortcuts.

## Acceptance and verification

- Follow read-only examples from a fresh test profile and validate script snippets with fixture/lab data.
- Record native platform support and unresolved distribution credentials honestly.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/058.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 058 --evidence codex/tracking/evidence/058.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 058 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
