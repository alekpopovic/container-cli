# 034 — Image inventory and inspection commands

- Phase: 04 Management
- Recommended reasoning: **medium**
- Depends on: **033**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 034`; resolve unfinished prerequisites if it refuses.

## Goal

Add image visibility using the same transport/output framework.

## Required implementation

1. Implement images ls and images inspect with full IDs, multiple tags/digests, size and creation data.
2. Deduplicate identity while retaining all tags; represent dangling images clearly and resolve ambiguous references safely.
3. Link images to referring containers where practical using bounded read snapshots.
4. Keep image pull/delete and registry credentials outside v1; sanitize sensitive image labels and human-rendered text.

## Acceptance and verification

- Test multi-tag and dangling images plus identical names on different hosts.
- Compare inventory references with the disposable daemon and JSON schema fixtures.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/034.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 034 --evidence codex/tracking/evidence/034.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 034 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
