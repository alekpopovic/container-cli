# 036 — Network inventory and attachments

- Phase: 04 Management
- Recommended reasoning: **medium**
- Depends on: **035**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 036`; resolve unfinished prerequisites if it refuses.

## Goal

Show Docker networking without making changes.

## Required implementation

1. Implement networks ls/inspect with driver, internal flag, IPAM and attached container relationships.
2. Support IPv4/IPv6, host/none modes and missing endpoints without fabricating addresses.
3. Use host-scoped IDs and safe text encoding for names/options; maintain consistent JSON types.
4. Keep network create/connect/disconnect/delete out of v1 and make unsupported commands explicit.

## Acceptance and verification

- Test bridge/host/none/IPv6 fixtures and an endpoint removed during inspection.
- Compare network attachment references with the disposable target.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/036.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 036 --evidence codex/tracking/evidence/036.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 036 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
