# 059 — Release candidate review and final defects

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **058**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 059`; resolve unfinished prerequisites if it refuses.

## Goal

Prepare a concrete release candidate ready for owner review.

## Required implementation

1. Review all milestones, architecture-specific native results, secret handling, CLI contracts and produced artifact hashes.
2. Fix release-blocking defects with focused regression checks; do not expand scope into a GUI or daemon.
3. Write release notes and known limitations including runtime prerequisites and exact signing status.
4. Prepare a local/draft release bundle tied to the tested commit, without publishing or contacting third parties.

## Acceptance and verification

- Run the required quality gate and final artifact smoke checks once after fixes.
- Verify every claimed platform and feature links to real evidence and no blocked core work is hidden.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/059.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 059 --evidence codex/tracking/evidence/059.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 059 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
