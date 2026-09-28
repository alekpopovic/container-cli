# 057 — Versioned schemas upgrade and compatibility policy

- Phase: 06 Quality and delivery
- Recommended reasoning: **medium**
- Depends on: **056**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 057`; resolve unfinished prerequisites if it refuses.

## Goal

Make upgrades predictable for existing scripts and profiles.

## Required implementation

1. Document v1 output-schema compatibility, stable exit codes and deprecation policy; avoid silently renaming machine fields.
2. Implement config version migration with backup and a clear rollback boundary.
3. Keep manual upgrades as default, with no mandatory network or self-update process; future automatic updates need a separate trust design.
4. Align binary version, release notes and generated help/examples with the final command set.

## Acceptance and verification

- Run old-config fixtures through migration and validate representative v1 JSON with compatibility tests.
- Check help/version and normal commands perform no unsolicited telemetry/update requests.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/057.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 057 --evidence codex/tracking/evidence/057.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 057 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
