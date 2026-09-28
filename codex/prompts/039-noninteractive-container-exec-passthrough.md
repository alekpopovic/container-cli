# 039 — Noninteractive container exec passthrough

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **038**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 039`; resolve unfinished prerequisites if it refuses.

## Goal

Run an explicit argv inside a container without consuming pipeline input.

## Required implementation

1. Implement exec TARGET -- COMMAND ARG... using Cobra ArgsLenAtDash and an exact target before --.
2. Require --host, --write and confirmation/--yes because any exec can mutate data; resolve a running container ID before execution.
3. Quote each container argv token remotely and pass stdin only with -i; keep stdout/stderr as passthrough streams and reject JSON/table output for exec.
4. Preserve the SSH-reported command exit status when available; status 255 is ambiguous with SSH failure. Wrapper errors keep documented application codes and clear diagnostics.

## Acceptance and verification

- Test exec -- /bin/false, payload from stdin, empty arguments and command flags beginning with dashes.
- Verify no confirmation reads payload stdin and quoted argv cannot append a command on the remote host.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/039.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 039 --evidence codex/tracking/evidence/039.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 039 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.
