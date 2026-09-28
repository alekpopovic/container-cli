# 056 — Installation distribution and optional signing

- Phase: 06 Quality and delivery
- Recommended reasoning: **medium**
- Depends on: **055**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 056`; resolve unfinished prerequisites if it refuses.

## Goal

Prepare clear installation paths without publishing automatically.

## Required implementation

1. Document checksum-verified archive installation into a user-owned PATH directory for Linux/macOS.
2. Prepare a Homebrew formula/tap or package recipe only as a reviewable artifact with real future release coordinates clearly unresolved until chosen.
3. Provide optional public macOS signing/notarization configuration using supplied credentials and official GoReleaser/Apple guidance.
4. Do not create provider accounts, silently upload binaries or claim signing verified without actual results; manual archives remain the baseline.

## Acceptance and verification

- Test installation/uninstall instructions against a temporary user path.
- Verify any unavailable signing credential is reported as unverified, while local build results remain accurate.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/056.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 056 --evidence codex/tracking/evidence/056.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 056 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
