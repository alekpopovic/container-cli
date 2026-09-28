# 003 — Command tree and argument validation

- Phase: 01 CLI foundation
- Recommended reasoning: **medium**
- Depends on: **002**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 003`; resolve unfinished prerequisites if it refuses.

## Goal

Implement the stable CLI grammar before adding transport.

## Required implementation

1. Declare hosts, containers, compose, images, volumes, networks, doctor, completion and version command groups with useful examples.
2. Implement shared --host, --ssh-config, --ssh-binary, --output, --timeout, --write, --yes and --no-color flags with command applicability checks.
3. Use Cobra argument validators and reject ambiguous or unused arguments; for exec reserve all arguments after -- for the container command.
4. Keep unimplemented commands explicit errors rather than empty successful output; support --help without loading remote configuration.

## Acceptance and verification

- Test valid and invalid subcommands, missing values, conflicting flags and arguments after --.
- Check no argument/flag error starts SSH or emits a successful data envelope.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/003.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 003 --evidence codex/tracking/evidence/003.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 003 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
