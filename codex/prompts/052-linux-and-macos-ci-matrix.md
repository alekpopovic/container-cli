# 052 — Linux and macOS CI matrix

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **051**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 052`; resolve unfinished prerequisites if it refuses.

## Goal

Configure native checks before native acceptance gates.

## Required implementation

1. Add GitHub Actions jobs for Linux and macOS, with clear amd64/arm64 build versus runtime coverage.
2. Use least-privilege tokens, reviewed/pinned dependencies and separate pull-request checks from trusted signing/publishing jobs.
3. Run unit/contract tests and create build artifacts/checksums without requiring signing secrets.
4. Make integration jobs opt-in on disposable runners and label unavailable host architectures unverified rather than inferring support.

## Acceptance and verification

- Validate workflow syntax and run available jobs, recording runner identities.
- Verify untrusted pull-request events cannot access signing credentials or publishing steps.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/052.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 052 --evidence codex/tracking/evidence/052.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 052 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
