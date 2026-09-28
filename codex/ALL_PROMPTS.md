# DockJump — all 60 Go/Cobra implementation prompts

Pack version 1.0.0 • 2026-09-28. Run one prompt at a time from the repository root.
Read AGENTS.md, CODEX_START.md, codex/docs/ARCHITECTURE.md and codex/docs/CLI_CONTRACT.md first.

# 001 — Repository audit and CLI contract

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **none**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 001`; resolve unfinished prerequisites if it refuses.

## Goal

Establish the Go/Cobra implementation baseline for DockJump.

## Required implementation

1. Inspect existing code, Git state and local instructions without overwriting unrelated work. Record the proposed app name DockJump and binary dockjump as working names.
2. Write docs/decisions/0001-cli.md: one Go module, Cobra command tree, native OpenSSH, remote Linux Docker Engine, Linux/macOS clients and no resident daemon.
3. Select a supported stable Go version and compatible Cobra dependencies using official sources, pin the module/toolchain and record the development environment.
4. Adopt codex/docs/CLI_CONTRACT.md, architecture and sequential milestone gates; list required test infrastructure honestly.

## Acceptance and verification

- Record repository/tool versions and conflicts with existing instructions.
- Confirm every proposed command uses the documented stdout/stderr, host-selection and permission rules.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/001.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 001 --evidence codex/tracking/evidence/001.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 001 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 002 — Go module and Cobra root command

- Phase: 01 CLI foundation
- Recommended reasoning: **medium**
- Depends on: **001**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 002`; resolve unfinished prerequisites if it refuses.

## Goal

Create a minimal runnable CLI without remote side effects.

## Required implementation

1. Initialize one Go module in the existing root; use a clearly local module path if no repository URL is provided and document renaming later.
2. Create cmd/dockjump/main.go and internal/cli constructors that return fresh Cobra command trees with injected streams/dependencies.
3. Add version and root help; use RunE and a top-level run function whose cleanup finishes before main calls os.Exit.
4. Commit go.mod/go.sum and a small Makefile or equivalent documented build/test commands; avoid generating unrelated service scaffolding.

## Acceptance and verification

- Run go test ./... and go build ./cmd/dockjump.
- Run help/version without SSH installed in the test environment and confirm they still work.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/002.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 002 --evidence codex/tracking/evidence/002.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 002 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

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


---

# 004 — Typed results errors and output contract

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **003**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 004`; resolve unfinished prerequisites if it refuses.

## Goal

Make the CLI dependable for jq and shell scripts.

## Required implementation

1. Define Go domain models and v1 JSON envelopes exactly as CLI_CONTRACT.md specifies; model typed errors with stable application exit codes.
2. Implement table/text output with color disabled off-TTY and versioned JSON/JSONL encoders using injected io.Writers.
3. Finite --output json must buffer a bounded result until success; send only one data document to stdout and diagnostics to stderr.
4. Limit raw output to explicitly supported passthrough/log commands. Streaming JSONL uses one complete object per line and reports later errors separately.

## Acceptance and verification

- Parse captured stdout as JSON and JSONL while deliberately emitting diagnostics.
- Test empty collections as [] and failures without partial finite JSON; verify exit codes in subprocess-level checks.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/004.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 004 --evidence codex/tracking/evidence/004.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 004 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 005 — Configuration profiles and precedence

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **004**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 005`; resolve unfinished prerequisites if it refuses.

## Goal

Store non-secret host metadata and predictable defaults.

## Required implementation

1. Use schema-versioned JSON in os.UserConfigDir()/dockjump/config.json with atomic writes, owner-only permissions and cross-process write locking.
2. Resolve ordinary preferences as explicit flags, documented DOCKJUMP_* environment variables, config defaults, then built-in defaults.
3. Keep --write and --yes invocation-only capabilities; never grant them implicitly from environment or a profile.
4. Store aliases, groups, labels, optional remote Docker context/binary path and default host; never store keys, passwords, container environment values or transcripts.

## Acceptance and verification

- Test flags versus environment versus config precedence and simultaneous profile updates.
- Exercise corrupt config recovery and preserve the original file rather than overwriting it silently.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/005.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 005 --evidence codex/tracking/evidence/005.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 005 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 006 — Policy enforcement and confirmation input

- Phase: 01 CLI foundation
- Recommended reasoning: **high**
- Depends on: **005**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 006`; resolve unfinished prerequisites if it refuses.

## Goal

Require deliberate target selection for remote changes.

## Required implementation

1. Implement service-level read versus mutation/exec policy; changing resources requires explicit --host and --write in this invocation.
2. Add --yes as confirmation bypass only after policy passes. For interactive confirmation read from /dev/tty, not piped stdin reserved for exec input.
3. Noninteractive calls lacking --yes fail promptly with the policy exit code, before dispatching a mutation.
4. Bind the operation plan to the resolved daemon identity and exact IDs; print a concise confirmation on stderr and revalidate immediately before execution.

## Acceptance and verification

- Call service operations without Cobra to prove the policy is not only a UI check.
- Test --yes without --write, implicit host on a write, and piped stdin preservation during confirmation.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/006.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 006 --evidence codex/tracking/evidence/006.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 006 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 007 — Execution interfaces and synthetic fixtures

- Phase: 01 CLI foundation
- Recommended reasoning: **medium**
- Depends on: **006**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 007`; resolve unfinished prerequisites if it refuses.

## Goal

Enable offline development through small testable Go interfaces.

## Required implementation

1. Define minimal interfaces for process execution, config storage, clock/limits, input confirmation and output rendering; keep command logic thin.
2. Create synthetic Docker inventory, inspect, logs, events and stats fixtures plus SSH failures and malformed/oversized output.
3. Label all fixture demonstrations explicitly and never fall back to fixture data after a failed real connection.
4. Add focused Go tests and temporary filesystem helpers; avoid package-level mutable command instances and init-time network calls.

## Acceptance and verification

- Run command construction repeatedly in one process and verify flags/state do not leak.
- Check offline tests spawn no SSH and real command errors never become demo success.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/007.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 007 --evidence codex/tracking/evidence/007.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 007 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 008 — Local doctor and deterministic startup

- Phase: 01 CLI foundation
- Recommended reasoning: **medium**
- Depends on: **007**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 008`; resolve unfinished prerequisites if it refuses.

## Goal

Diagnose local prerequisites without requiring Docker locally.

## Required implementation

1. Implement doctor local for OS/architecture, executable location, version, config permissions and agent-socket availability.
2. Prefer a validated absolute native SSH path, normally /usr/bin/ssh, with an explicit override; do not execute a shell to discover PATH.
3. Resolve home/config paths with Go OS APIs and support spaces/non-ASCII paths; keep startup free of unrequested host probes.
4. Document that the installed app requires its binary plus native OpenSSH; Python is only the optional development prompt tracker.

## Acceptance and verification

- Test missing/non-executable SSH override, minimal PATH and missing agent socket.
- Confirm version, help and shell-completion script generation remain available without SSH.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/008.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 008 --evidence codex/tracking/evidence/008.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 008 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 009 — SSH config candidate discovery

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **008**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 009`; resolve unfinished prerequisites if it refuses.

## Goal

Discover concrete aliases without reimplementing OpenSSH.

## Required implementation

1. Implement hosts discover using ~/.ssh/config or a chosen config; follow Include paths with bounded size/depth and cycle detection.
2. Extract literal Host aliases including multiple names per line while excluding wildcards/negation from concrete candidates.
3. Support manually adding an alias when configuration is dynamic or cannot be enumerated; no automatic connection scan.
4. Never modify SSH config and do not evaluate Match exec/ProxyCommand during discovery; report partial discovery clearly.

## Acceptance and verification

- Test quoted include paths, includes with spaces, comments, multiple aliases, glob patterns and cycles.
- Verify discovery invokes no subprocess and never treats a wildcard as a concrete host.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/009.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 009 --evidence codex/tracking/evidence/009.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 009 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 010 — Effective SSH resolution and trust boundary

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **009**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 010`; resolve unfinished prerequisites if it refuses.

## Goal

Use native OpenSSH for actual connection semantics.

## Required implementation

1. Resolve one explicitly selected alias with bounded ssh -G output, preserving the same executable and optional -F config for subsequent calls.
2. Validate alias syntax against a documented conservative grammar and reject leading dashes, whitespace and control characters.
3. Retain the original alias for connections and show effective host/user/port/jump data as diagnostics.
4. Treat user SSH config as trusted executable configuration: ssh -G can run Match exec. Never execute downloaded/unselected configs to populate completions.

## Acceptance and verification

- Compare diagnostics with native ssh -G on synthetic trusted configs with overlapping Host rules.
- Verify bad aliases fail before process creation and config paths are passed as arguments.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/010.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 010 --evidence codex/tracking/evidence/010.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 010 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 011 — Go OpenSSH process runner

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **010**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 011`; resolve unfinished prerequisites if it refuses.

## Goal

Implement bounded subprocess execution and reliable cleanup.

## Required implementation

1. Use os/exec with explicit argument slices and context deadlines; read stdout/stderr concurrently through bounded readers.
2. Own and reap every child. CommandContext alone may not clean descendants; implement a Unix process-group cancellation strategy for noninteractive operations and finite pipe-drain waits.
3. Separate snapshot deadlines, connection deadlines and long-lived stream cancellation; never impose the default snapshot timeout on log follow.
4. Do not invoke a local shell; provide structured exit/error metadata to higher layers without exposing raw secrets.

## Acceptance and verification

- Use inert helper processes to test large stderr, held-open descendant pipes, cancellation and nonzero exit.
- Verify no goroutine/child leak remains after timeout or consumer cancellation.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/011.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 011 --evidence codex/tracking/evidence/011.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 011 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 012 — Remote command validation and quoting

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **011**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 012`; resolve unfinished prerequisites if it refuses.

## Goal

Encode remote argv safely across the OpenSSH shell boundary.

## Required implementation

1. Create typed Docker operation builders plus a central POSIX single-quote encoder for every remote argument.
2. Document that local os/exec argument slices do not remove the remote shell. Target POSIX-compatible remote login shells and report unsupported setups.
3. Validate IDs, numeric bounds, fixed flags and optional absolute remote paths before quoting; templates such as {{json .}} remain literal.
4. For later exec passthrough, accept explicit user argv after -- and quote each token; never substitute remote labels or config metadata as shell code.

## Acceptance and verification

- Test empty strings, quotes, semicolons, backticks, dollar substitution, newlines and option-like values against an inert shell harness.
- Confirm one user argv token stays one remote token and cannot append a host-side command.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/012.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 012 --evidence codex/tracking/evidence/012.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 012 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 013 — Keys host verification and unattended jumps

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **012**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 013`; resolve unfinished prerequisites if it refuses.

## Goal

Reuse existing SSH credentials with clear setup failures.

## Required implementation

1. Use IdentityFile, SSH agent and known_hosts through native SSH with strict host-key checking; background probes must not request passwords.
2. Support a documented terminal bootstrap for missing host trust with independent fingerprint verification and key loading in the OS agent.
3. Test jump-host and destination credential handling separately; destination CLI options are not assumed to configure a ProxyJump child.
4. Do not persist passphrases, enable agent forwarding or edit trust files; place credential diagnostics on stderr and keep stdout machine-readable.

## Acceptance and verification

- Test unknown/changed host keys and unavailable encrypted keys on a disposable lab.
- Verify missing jump credentials fail or time out promptly without hidden input prompts.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/013.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 013 --evidence codex/tracking/evidence/013.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 013 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 014 — Invocation-scoped connection reuse

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **013**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 014`; resolve unfinished prerequisites if it refuses.

## Goal

Reuse connections during one command without adding a resident daemon.

## Required implementation

1. Create an app-owned control master/socket when one invocation runs several reads, watch loops or a group query.
2. Keep private socket paths short and mode-restricted on Linux/macOS; use unique ownership per invocation and bounded persistence.
3. For single operations permit a simpler path; explicitly avoid taking ownership of a user control master from their config.
4. Clean up only owned processes/sockets on completion and provide a bounded fallback if multiplexing is unavailable.

## Acceptance and verification

- Test simultaneous invocations, stale sockets and long home paths.
- Confirm one command exiting cannot close another invocation or the users existing SSH master.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/014.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 014 --evidence codex/tracking/evidence/014.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 014 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 015 — Remote Docker capability and endpoint probe

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **014**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 015`; resolve unfinished prerequisites if it refuses.

## Goal

Identify the exact daemon that subsequent commands will affect.

## Required implementation

1. Probe remote client/server versions, daemon identity, Linux OS support, permissions and Compose plugin availability with bounded JSON output.
2. Use the remote users default Docker endpoint or an explicit named remote context consistently on every operation.
3. Show SSH destination separately from actual Docker endpoint, including rootless sockets or contexts that point elsewhere.
4. Support an explicit fixed sudo -n Docker mode for an already configured remote policy. Never install sudo rules, request passwords or change server access.

## Acceptance and verification

- Test missing Docker, socket denial, stopped daemon, rootless context, Compose absence and sudo needing a password.
- Verify selected context, binary and privilege mode propagate to every command builder.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/015.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 015 --evidence codex/tracking/evidence/015.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 015 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 016 — Hosts profiles and checks

- Phase: 02 Native SSH
- Recommended reasoning: **medium**
- Depends on: **015**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 016`; resolve unfinished prerequisites if it refuses.

## Goal

Connect saved metadata and SSH probes to usable commands.

## Required implementation

1. Implement hosts add/show/list/remove/use and hosts check, with explicit local versus remote behavior in help.
2. Use SSH aliases as profile keys, separate display labels/groups and allow reads through a configured default host.
3. Keep hosts list local unless an explicit check action is requested; removing a profile must not alter ~/.ssh/config or remote resources.
4. Display effective destination and daemon identity through the documented output encoders; sanitize diagnostic fields.

## Acceptance and verification

- Exercise add/use/list/remove with two identical display labels and distinct aliases.
- Confirm write operations still demand an explicit --host despite a saved default.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/016.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 016 --evidence codex/tracking/evidence/016.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 016 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 017 — Structured SSH error classification

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **016**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 017`; resolve unfinished prerequisites if it refuses.

## Goal

Provide useful error codes without pretending every SSH failure is perfectly classifiable.

## Required implementation

1. Map configuration, timeout, transport/authentication, permission, remote command and unknown-outcome failures to the CLI error contract.
2. Keep bounded sanitized stderr excerpts with stage information; use conservative fallback classification for locale-dependent OpenSSH messages.
3. Return JSON diagnostics on stderr in structured output modes and concise text otherwise, with no duplicated Cobra error/usage blocks.
4. Implement a hosts check walkthrough that reports stages and remaining setup without dumping keys or full config.

## Acceptance and verification

- Test non-English/unknown stderr, SSH status 255, remote Docker nonzero and malformed JSON independently.
- Ensure all paths return nonzero and stdout remains empty for a failed finite request.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/017.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 017 --evidence codex/tracking/evidence/017.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 017 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 018 — Real SSH vertical slice checkpoint

- Phase: 02 Native SSH
- Recommended reasoning: **high**
- Depends on: **017**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 018`; resolve unfinished prerequisites if it refuses.

## Goal

Prove a direct and ProxyJump Docker read before expanding the CLI.

## Required implementation

1. Use disposable client/bastion/private-target resources with temporary keys and strict dedicated known_hosts.
2. Run hosts check and a bounded remote Docker JSON probe with no Docker or jq on the client.
3. Document topology, versions, commands, actual results and process cleanup in docs/checkpoints/018-ssh.md.
4. Record missing native platforms separately; mock transport results cannot satisfy this live SSH gate.

## Acceptance and verification

- Demonstrate the private target is accessed through the jump host and the CLI cleans its owned sessions.
- If a real disposable SSH target is unavailable, retain the checkpoint as blocked with the exact next prerequisite.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/018.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 018 --evidence codex/tracking/evidence/018.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 018 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 019 — Container inventory adapter

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **018**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 019`; resolve unfinished prerequisites if it refuses.

## Goal

Normalize Docker JSON lines into a stable container model.

## Required implementation

1. Implement containers ls using docker ps -a --no-trunc with a JSON template through the central builder.
2. Preserve full IDs, names, image, state, display status and ports while tolerating absent optional/new fields.
3. Distinguish successful zero rows from malformed output, banners, nonzero exit or a truncated response.
4. Bound total bytes, records and per-record decoding; do not silently discard arbitrary output lines.

## Acceptance and verification

- Test empty, running, exited, restarting and malformed fixtures with Unicode values.
- Compare normalized count/IDs with the disposable target CLI.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/019.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 019 --evidence codex/tracking/evidence/019.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 019 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 020 — Container filtering and stable presentation

- Phase: 03 Read-only MVP
- Recommended reasoning: **medium**
- Depends on: **019**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 020`; resolve unfinished prerequisites if it refuses.

## Goal

Make inventory useful in terminals and shell pipelines.

## Required implementation

1. Add --all, supported --filter values, --sort and --quiet with precisely defined semantics; default listing shows running containers and --all includes stopped.
2. Render safe human tables using a stable column order and no ANSI in pipes; --quiet emits full IDs one per line and conflicts with structured output flags.
3. JSON uses the documented items envelope and stable types, independent of terminal width.
4. Resolve target names for later detail operations by exact match or unique ID prefix on the selected daemon; ambiguity fails with a useful error.

## Acceptance and verification

- Test duplicate-looking names across hosts, ambiguous prefixes and malformed filters.
- Pipe JSON through a parser and verify human formatting never modifies machine fields.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/020.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 020 --evidence codex/tracking/evidence/020.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 020 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 021 — Container inspect and secret masking

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **020**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 021`; resolve unfinished prerequisites if it refuses.

## Goal

Expose accurate details without revealing secrets by default.

## Required implementation

1. Implement containers inspect TARGET through inspect JSON arrays and normalize mounts, health, exit data, network addresses and restart configuration.
2. Mask all environment values and likely-sensitive label values before output encoding; keep inspect result copies out of app diagnostics.
3. Support explicit --show-secrets only for the requested result, with a warning on stderr and no persistence.
4. Handle container disappearance, absent health, null fields and large data within documented bounds.

## Acceptance and verification

- Test stopped, healthcheck-free, rootless and multi-network fixtures.
- Scan default stdout/stderr and local files for seeded environment/label secrets.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/021.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 021 --evidence codex/tracking/evidence/021.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 021 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 022 — Focused container detail subcommands

- Phase: 03 Read-only MVP
- Recommended reasoning: **medium**
- Depends on: **021**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 022`; resolve unfinished prerequisites if it refuses.

## Goal

Offer concise health ports mounts and environment views.

## Required implementation

1. Implement containers health, ports, mounts and env from the shared inspect model rather than new shell parsing.
2. Differentiate running state from health configuration; distinguish exposed ports from host bindings including IPv6 and multiple bindings.
3. Keep env values masked unless --show-secrets is explicit; escape terminal control characters in human tables.
4. Use the same finite JSON envelope with command-appropriate items and clear empty/not-configured states.

## Acceptance and verification

- Check field consistency across inspect and detail commands.
- Exercise long paths, IPv6 ports, missing health and control characters without terminal escapes.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/022.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 022 --evidence codex/tracking/evidence/022.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 022 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 023 — Bounded log snapshots

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **022**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 023`; resolve unfinished prerequisites if it refuses.

## Goal

Retrieve recent logs with correct output and stream attribution limits.

## Required implementation

1. Implement containers logs with a bounded positive --tail, timestamps and validated --since/--until values.
2. Human output is safe text by default, JSON is a bounded records envelope and --output raw is an explicit byte passthrough mode.
3. Application stderr and SSH/Docker diagnostics share the SSH stderr channel; record channel provenance honestly instead of claiming definitive application-origin attribution.
4. Use process status for final failure, handle unavailable logging drivers and invalid UTF-8 and disclose truncation.

## Acceptance and verification

- Test application stderr, daemon errors, invalid UTF-8 and oversized lines.
- Verify finite JSON produces no partial stdout if the operation fails.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/023.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 023 --evidence codex/tracking/evidence/023.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 023 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 024 — Log follow backpressure and cancellation

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **023**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 024`; resolve unfinished prerequisites if it refuses.

## Goal

Support long-lived log streams in both terminals and pipelines.

## Required implementation

1. Add containers logs --follow with safe text or JSONL records; reject one-document JSON for an unbounded stream.
2. Use bounded queues, finite line sizes and explicit dropped/truncated markers when the consumer cannot keep up.
3. Stop remote child activity on context cancellation, stdout write failure or a closed downstream pipe; do not retain an ever-growing log history.
4. If an optional read reconnect is enabled, mark gaps and best-effort timestamp resumption; never claim a lossless log transport.

## Acceptance and verification

- Run a high-rate synthetic stream into a slow/closed writer and verify bounded memory/child cleanup.
- Send SIGINT during live log follow and confirm controlled termination with a documented status.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/024.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 024 --evidence codex/tracking/evidence/024.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 024 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 025 — Container statistics snapshots

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **024**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 025`; resolve unfinished prerequisites if it refuses.

## Goal

Provide normalized metrics suitable for inspection and scripts.

## Required implementation

1. Implement containers stats with --once default and bounded docker stats --no-stream --no-trunc JSON-template requests.
2. Normalize unit strings while retaining appropriate raw display values; CPU may exceed 100 and memory follows Docker CLI reporting semantics.
3. Represent missing/stopped samples as unavailable, not measured zero; label sample timestamps and daemon identity.
4. Restrict one inflight stats snapshot per host and avoid one subprocess per displayed row.

## Acceptance and verification

- Test KiB/MiB/GiB conversion, unknown values, CPU over 100 and container disappearance.
- Compare selected samples with native Docker CLI output on the lab target.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/025.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 025 --evidence codex/tracking/evidence/025.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 025 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 026 — Watch mode for inventory and statistics

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **025**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 026`; resolve unfinished prerequisites if it refuses.

## Goal

Add lightweight terminal monitoring without a full-screen framework.

## Required implementation

1. Implement containers ls --watch and containers stats --watch with validated --interval and --count for bounded automation.
2. For TTY human output, update readable screens conservatively; for non-TTY emit successive labeled text snapshots or explicit JSONL, with no ANSI screen controls.
3. Use one invocation-owned SSH connection and one inflight refresh, bounded buffers and cancellation between samples.
4. Reject --output json for multi-snapshot modes, stop when count is reached and surface stale/failed samples instead of empty success.

## Acceptance and verification

- Use fake clocks to test interval/count, slow snapshots and cancellation.
- Verify redirected watch output is parseable JSONL when requested and contains no terminal escape sequences.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/026.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 026 --evidence codex/tracking/evidence/026.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 026 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 027 — Docker events command

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **026**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 027`; resolve unfinished prerequisites if it refuses.

## Goal

Expose a bounded event stream with explicit gap semantics.

## Required implementation

1. Implement events with validated filters and --since/--until using Docker event JSON output; support safe text and JSONL.
2. Distinguish events from complete inventory and note daemon retention/reconnect gaps; use a fresh snapshot for reconciliation.
3. Enforce record/output bounds and reuse signal, writer-error and child cleanup handling.
4. Keep event command execution explicit, with no persistent background collector or unselected host subscriptions.

## Acceptance and verification

- Test duplicate/burst/missing-field events and a reconnect gap.
- Confirm a downstream pipe closing immediately does not leave an SSH process behind.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/027.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 027 --evidence codex/tracking/evidence/027.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 027 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 028 — Read retries timeout and cancellation policy

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **027**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 028`; resolve unfinished prerequisites if it refuses.

## Goal

Make slow connections predictable across all read commands.

## Required implementation

1. Centralize bounded retry/backoff for idempotent read setup with stage-specific deadlines and jitter.
2. Do not retry a finite request after writing a successful/partial result to stdout unless its output protocol explicitly represents retries.
3. Treat log/event reconnection as opt-in and report gaps; keep stream lifetime separate from snapshot timeout.
4. Add elapsed timings and actionable errors on stderr without changing deterministic result schemas.

## Acceptance and verification

- Simulate network loss before and after output and assert the contract for each mode.
- Check that no mutation or exec command can use automatic retry code paths.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/028.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 028 --evidence codex/tracking/evidence/028.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 028 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 029 — Compose discovery and service grouping

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **028**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 029`; resolve unfinished prerequisites if it refuses.

## Goal

Group remote containers without requiring local Compose files.

## Required implementation

1. Implement compose ls using remote Compose JSON when available and label-derived grouping as a read fallback.
2. Implement compose ps PROJECT using selected-daemon containers and service labels; support projects with multiple service instances.
3. Do not treat reported ConfigFiles/working-directory labels as trusted executable configuration or local paths.
4. Distinguish plugin absence from no projects and keep project identity scoped to host/daemon.

## Acceptance and verification

- Test absent plugin, duplicate service names in different projects and deleted containers.
- Verify no local project files are required for read-only grouping.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/029.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 029 --evidence codex/tracking/evidence/029.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 029 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 030 — Read-only CLI MVP checkpoint

- Phase: 03 Read-only MVP
- Recommended reasoning: **high**
- Depends on: **029**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 030`; resolve unfinished prerequisites if it refuses.

## Goal

Deliver a usable command-line Docker inspector.

## Required implementation

1. Integrate hosts, list/inspect/detail commands, logs, stats/watch, events and Compose grouping into consistent help and examples.
2. Run direct and ProxyJump workflows against disposable infrastructure using both human and structured output.
3. Review stdout/stderr separation, empty results, bounds, signal cleanup and explicit target identity.
4. Write docs/checkpoints/030-read-only.md with real commands, statuses and remaining native-platform checks.

## Acceptance and verification

- Demonstrate list JSON consumed by jq or a JSON parser, live logs cancellation and bounded stats watch.
- Keep this checkpoint blocked if only fixture output is available for the integrated remote workflow.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/030.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 030 --evidence codex/tracking/evidence/030.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 030 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 031 — Mutation planning dry-run and activity records

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **030**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 031`; resolve unfinished prerequisites if it refuses.

## Goal

Plan remote changes explicitly without replaying uncertain actions.

## Required implementation

1. Implement invocation-scoped mutation plans containing host/daemon identity, operation and resolved target IDs, enforcing --host plus --write.
2. Add --dry-run to mutating commands: perform necessary reads and output the plan, dispatch no mutation, still enforce explicit target/write intent.
3. Require /dev/tty confirmation or --yes for execution; revalidate target identity and serialize conflicting operations.
4. Record bounded sanitized local activity if enabled, classifying transport loss after dispatch as unknown outcome; this is not a tamper-proof audit trail.

## Acceptance and verification

- Test plan/execute target changes, non-TTY confirmation and dry-run with a spy transport.
- Verify dropped responses are never interpreted as permission to automatically retry.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/031.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 031 --evidence codex/tracking/evidence/031.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 031 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 032 — Container start stop and restart commands

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **031**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 032`; resolve unfinished prerequisites if it refuses.

## Goal

Implement essential lifecycle actions with outcome verification.

## Required implementation

1. Add containers start/stop/restart for explicit target lists using current full IDs and fixed command builders.
2. Support bounded --stop-timeout where applicable and validate it before contacting the daemon.
3. Return action results through human tables or JSON and perform a read to verify observed post-action state without claiming application readiness.
4. After a lost response return the unknown-outcome contract and advise a read refresh; never repeat the action automatically.

## Acceptance and verification

- Run lifecycle actions on disposable containers and compare final state.
- Interrupt connectivity after submission and confirm one dispatch plus an unknown outcome.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/032.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 032 --evidence codex/tracking/evidence/032.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 032 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 033 — Batch outcomes and stopped-container removal

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **032**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 033`; resolve unfinished prerequisites if it refuses.

## Goal

Handle small explicit batches with honest partial results.

## Required implementation

1. Bound target counts and action concurrency; preserve per-target outcomes and show partial failures without rolling back successful remote actions automatically.
2. Implement containers rm only for explicitly selected currently stopped containers; support neither force nor volume-removal flags.
3. Recheck state/identity before removal and report a race safely if a container restarts.
4. Return exit 7 for partial batch failures and 8 when any dispatched outcome is unknown; include all known per-target results.

## Acceptance and verification

- Test success/failure/unknown combinations and cancellation of queued targets.
- Verify running-container removal is refused and volumes are never deleted by this command.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/033.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 033 --evidence codex/tracking/evidence/033.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 033 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

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


---

# 035 — Volume inventory and references

- Phase: 04 Management
- Recommended reasoning: **medium**
- Depends on: **034**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 035`; resolve unfinished prerequisites if it refuses.

## Goal

Make storage relationships visible without reading volume data.

## Required implementation

1. Implement volumes ls/inspect with driver, scope, safe options and current container mount references.
2. Treat no observed references as a snapshot fact rather than proof deletion is safe.
3. Handle remote drivers, missing mountpoint and disappearing container references gracefully.
4. Keep volume content browsing, deletion and prune outside this release; preserve permission and output conventions.

## Acceptance and verification

- Test named/anonymous/external-driver volumes and absent optional fields.
- Verify volume commands never execute a remote filesystem read of volume contents.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/035.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 035 --evidence codex/tracking/evidence/035.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 035 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

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


---

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


---

# 038 — Management CLI MVP checkpoint

- Phase: 04 Management
- Recommended reasoning: **high**
- Depends on: **037**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 038`; resolve unfinished prerequisites if it refuses.

## Goal

Verify readable outcomes and policy across the management command set.

## Required implementation

1. Demonstrate read-only defaults, dry-run, confirmation, --yes and lifecycle/Compose actions against a disposable jump-host target.
2. Review resource list/inspect commands and action help for consistent target and output semantics.
3. Exercise stale selection, partial failure, changed daemon identity and unknown mutation outcome.
4. Write docs/checkpoints/038-management.md with actual results and unresolved compatibility gates.

## Acceptance and verification

- Call services directly to confirm mutations cannot bypass policy through another command path.
- Validate action JSON and exit codes in shell scripts; block missing live functionality rather than declaring fixture success.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/038.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 038 --evidence codex/tracking/evidence/038.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 038 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

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


---

# 040 — Interactive exec with native terminal behavior

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **039**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 040`; resolve unfinished prerequisites if it refuses.

## Goal

Support a real shell through the users existing terminal.

## Required implementation

1. Add -t and -i behavior with explicit TTY detection; -t requires an actual local terminal and -it is the normal interactive shell form.
2. Attach local terminal file descriptors directly to native OpenSSH, request a remote PTY and run Docker exec with the matching flags.
3. Prefer native SSH to own local terminal mode/resize rather than embedding a full terminal emulator; define signal ownership to avoid double forwarding.
4. Restore any state changed by DockJump on every ordinary error/cancel path; report missing shells and exited containers cleanly.

## Acceptance and verification

- Use a PTY-based integration harness to check Ctrl-C, resize, shell exit and abrupt SSH failure.
- Verify terminal settings are restored afterward and structured read commands never allocate a PTY.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/040.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 040 --evidence codex/tracking/evidence/040.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 040 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 041 — Signals process groups and broken pipes

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **040**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 041`; resolve unfinished prerequisites if it refuses.

## Goal

Make termination reliable for terminals and automation.

## Required implementation

1. Integrate SIGINT/SIGTERM handling with contexts and bounded owned-process shutdown for snapshot/stream commands.
2. Keep interactive exec foreground/terminal handling separate; do not put it into a detached process session that loses its controlling terminal.
3. Handle writer errors and EPIPE deliberately, documenting exit 141 for a closed data pipe after cleanup and 130/143 for wrapper cancellation.
4. Ensure cleanup precedes final exit and raw exec preserves child status where available without duplicate signal delivery.

## Acceptance and verification

- Run subprocess tests for SIGINT/SIGTERM and piping large output to head.
- Verify child/goroutine counts return to baseline and no cleanup is skipped by an early os.Exit.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/041.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 041 --evidence codex/tracking/evidence/041.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 041 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 042 — Read-only host groups and fleet snapshots

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **041**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 042`; resolve unfinished prerequisites if it refuses.

## Goal

Query selected host groups with bounded concurrency.

## Required implementation

1. Add hosts group metadata and a mutually exclusive --group selector for finite supported read commands only.
2. Execute a bounded number of host reads, with identity on every row/item and stable final ordering for finite JSON.
3. Represent individual host errors and exit 7 for partial fleet results; do not substitute cached/empty data for a failed host.
4. Reject groups for mutations, exec and infinite streams in v1 to keep targeting clear.

## Acceptance and verification

- Test one successful and one denied host, shared container names and deterministic JSON order.
- Measure concurrency against configured bounds and verify no host outside the selected group is contacted.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/042.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 042 --evidence codex/tracking/evidence/042.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 042 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 043 — Shell completion and generated reference docs

- Phase: 05 Exec and automation
- Recommended reasoning: **medium**
- Depends on: **042**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 043`; resolve unfinished prerequisites if it refuses.

## Goal

Provide useful completion without unexpected remote execution.

## Required implementation

1. Generate bash, zsh and fish completion scripts with Cobra and document user-installed locations.
2. Complete command/flag names and saved literal host aliases locally; dynamic container completion uses only explicitly cached local metadata if available.
3. Do not run ssh -G, connect to servers or read private keys during completion; allow local completion to fail quietly if config is absent.
4. Generate reference Markdown/man pages from the implemented command tree and keep examples aligned with actual flags.

## Acceptance and verification

- Smoke-test completion generation and shell syntax on available shells.
- Instrument completion to prove no transport call occurs and verify help generation is deterministic.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/043.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 043 --evidence codex/tracking/evidence/043.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 043 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 044 — Configuration portability and diagnostics export

- Phase: 05 Exec and automation
- Recommended reasoning: **medium**
- Depends on: **043**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 044`; resolve unfinished prerequisites if it refuses.

## Goal

Help users move non-secret preferences between Linux and macOS.

## Required implementation

1. Implement config path/show and sanitized export/import with schema validation, previews and explicit target file selection.
2. Separate application config from SSH config; importing metadata never imports keys, trust files, scripts or automatic write privileges.
3. Extend doctor with redacted support output, platform/tool versions and error stages; do not include raw logs, exec input or environment secrets.
4. Lock writes and preserve backups for concurrent commands or schema migration failures.

## Acceptance and verification

- Test paths with spaces/non-ASCII characters, missing home/config dirs and corrupt imports.
- Scan diagnostic exports for seeded secrets and verify import does not alter SSH files.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/044.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 044 --evidence codex/tracking/evidence/044.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 044 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 045 — Performance bounds and streaming stress

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **044**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 045`; resolve unfinished prerequisites if it refuses.

## Goal

Measure the CLI under large inventories and output pressure.

## Required implementation

1. Benchmark decoding/rendering 1000 containers, large inspect records and high-rate logs/events on a documented machine.
2. Set explicit byte/record/queue/concurrency limits and test long lines beyond bufio.Scanner defaults; do not rely on default token sizes silently.
3. Prefer incremental JSONL streaming and bounded finite snapshots; avoid retaining full history or one subprocess per row.
4. Record actual wall time, peak memory and process/goroutine behavior without inventing cross-platform benchmark numbers.

## Acceptance and verification

- Confirm memory reaches a bound under a slow writer and cancellation closes owned children.
- Measure finite output and record performance limits in docs/benchmarks.md.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/045.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 045 --evidence codex/tracking/evidence/045.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 045 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 046 — Feature-complete CLI checkpoint

- Phase: 05 Exec and automation
- Recommended reasoning: **high**
- Depends on: **045**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 046`; resolve unfinished prerequisites if it refuses.

## Goal

Review the complete CLI before packaging.

## Required implementation

1. Run the end-to-end host discovery/check, read commands, lifecycle, Compose, exec, completion and scripting journey.
2. Remove misleading placeholder commands, stale help and inconsistent flag behavior.
3. Verify there is no resident service, GUI dependency or local Docker requirement in the built CLI.
4. Write docs/checkpoints/046-feature-complete.md with verified features and explicit remaining platform/build gates.

## Acceptance and verification

- Run both human and structured workflows on the current native OS against the disposable target.
- Block any missing core feature; distinguish upcoming native-platform acceptance from completed functionality.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/046.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 046 --evidence codex/tracking/evidence/046.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 046 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 047 — Focused transport and CLI security review

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **046**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 047`; resolve unfinished prerequisites if it refuses.

## Goal

Review the real boundaries implemented by DockJump.

## Required implementation

1. Audit local exec arguments, remote quoting, alias/config trust, host verification, control socket ownership and service-level write policy.
2. Test malicious labels/log escapes, argv injection, leading-dash aliases, untrusted include paths and target identity races.
3. Check default secret masking, exported diagnostics, exec boundaries and group restrictions.
4. Document concrete findings and fixes; read-only CLI policy is not a server-side restriction on a Docker-privileged account.

## Acceptance and verification

- Run targeted regression checks for every found defect.
- Verify no command disables host verification, exposes Docker TCP or automatically changes remote privileges.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/047.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 047 --evidence codex/tracking/evidence/047.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 047 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 048 — Command contract and regression suite

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **047**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 048`; resolve unfinished prerequisites if it refuses.

## Goal

Lock down important behavior through external observable tests.

## Required implementation

1. Build table-driven tests for argument validation, config precedence, writer separation, exit codes and output schemas.
2. Cover empty/malformed Docker responses, masking, partial results, interrupted writes and remote command quoting.
3. Use fresh Cobra instances with injected dependencies and subprocess tests where actual process exit/signals matter.
4. Avoid snapshots that merely repeat implementation; tests should fail on a realistic contract regression.

## Acceptance and verification

- Run go test ./... and a race-enabled test run where supported.
- Demonstrate one controlled regression is caught, restore it and record exact check results.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/048.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 048 --evidence codex/tracking/evidence/048.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 048 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 049 — Fuzzing parsers and argument encoding

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **048**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 049`; resolve unfinished prerequisites if it refuses.

## Goal

Explore hostile input at the narrowest high-risk boundaries.

## Required implementation

1. Add focused Go fuzz targets for JSON-line decoding, bounded log records, alias validation and remote argument quoting.
2. Seed realistic examples including quotes, control characters, invalid UTF-8 and very long fields.
3. Assert bounded work/no panic, semantic argument preservation where applicable, and no accidental mutation or subprocess during pure parsing.
4. Use finite local fuzz budgets and preserve small reproductions of any discovered defects.

## Acceptance and verification

- Run each fuzz target for a documented bounded duration on the available platform.
- Record coverage limits and fix any reproducible crash before proceeding.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/049.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 049 --evidence codex/tracking/evidence/049.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 049 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 050 — Disposable SSH and Docker integration lab

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **049**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 050`; resolve unfinished prerequisites if it refuses.

## Goal

Provide reproducible live transport tests without production access.

## Required implementation

1. Create an opt-in client/bastion/private-target lab with ephemeral keys, strict known_hosts and a dedicated disposable Docker daemon.
2. Keep the private target inaccessible directly from the client and test direct access using a separate intended alias.
3. Cover inventory, logs, stats, mutations, Compose, exec, host-key changes, rootless access where available and reconnect/cleanup.
4. Avoid mounting the users production Docker socket; any privileged Docker-in-Docker setup belongs in a dedicated disposable test VM with explicit cleanup.

## Acceptance and verification

- Run the suite and show the ProxyJump route plus failure isolation.
- Verify key/process/container cleanup and document exact OS/OpenSSH/Docker versions.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/050.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 050 --evidence codex/tracking/evidence/050.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 050 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 051 — Local quality gate and dependency checks

- Phase: 06 Quality and delivery
- Recommended reasoning: **medium**
- Depends on: **050**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 051`; resolve unfinished prerequisites if it refuses.

## Goal

Give contributors one reproducible verification path.

## Required implementation

1. Create gofmt-check, go vet, unit tests, applicable race tests, build and optional integration/fuzz targets in a simple Makefile.
2. Run govulncheck with a pinned compatible tool version when available and distinguish unavailable checks from passing results.
3. Use go.mod/go.sum and a pinned Go toolchain; record CGO settings and OS-specific files explicitly.
4. Do not download/update toolchains silently during an offline verification claim; record network requirements and missing dependencies.

## Acceptance and verification

- Run the standard gate from a clean dependency state where possible.
- Temporarily fail one required check to verify the aggregate command returns nonzero, then restore the code.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/051.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 051 --evidence codex/tracking/evidence/051.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 051 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

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


---

# 053 — GoReleaser and cross-platform binary archives

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **052**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 053`; resolve unfinished prerequisites if it refuses.

## Goal

Produce reproducible CLI artifacts for the four target OS/architecture pairs.

## Required implementation

1. Configure the current compatible GoReleaser schema for linux/darwin and amd64/arm64, starting with CGO_ENABLED=0 if dependencies permit.
2. Embed version/commit metadata and use release-tag/source-date inputs for reproducibility; do not require a GUI packaging toolchain.
3. Create tar.gz archives containing the binary, license if chosen, README, completion scripts and checksums.
4. Run snapshot builds locally without publishing and document any dependencies that prevent a pure-Go build.

## Acceptance and verification

- Build all configured targets and inspect archive paths/permissions and binary metadata.
- Record cross-build success separately from native runtime verification.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/053.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 053 --evidence codex/tracking/evidence/053.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 053 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 054 — Linux native binary acceptance

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **053**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 054`; resolve unfinished prerequisites if it refuses.

## Goal

Verify real operation of Linux artifacts.

## Required implementation

1. Install the generated Linux binary in a temporary user path on the documented baseline and run help/version/doctor without development tools.
2. Run direct and ProxyJump list/logs/stats, mutation and exec workflows with strict known_hosts.
3. Test signals, downstream closed pipes, completion and config paths; inspect child processes after exit.
4. Record amd64 and arm64 runtime results separately, including the exact runner/VM/physical environment; keep untested targets explicit.

## Acceptance and verification

- Execute the native smoke suite for every Linux architecture claimed verified.
- If a required native environment is missing, keep its acceptance blocked with a precise setup action.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/054.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 054 --evidence codex/tracking/evidence/054.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 054 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

# 055 — macOS native binary acceptance

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **054**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 055`; resolve unfinished prerequisites if it refuses.

## Goal

Verify Apple Silicon and Intel terminal workflows.

## Required implementation

1. Run the packaged darwin binaries natively on documented supported macOS versions and record architecture/tool versions.
2. Test /usr/bin/ssh, agent/encrypted-key access, path handling, strict host trust and jump connectivity.
3. Exercise logs/watch cancellation, exec resize/Ctrl-C and completion in zsh plus bash where supported.
4. Keep unsigned local execution and public signing/notarization status separate; no Gatekeeper bypass is the prescribed installation flow.

## Acceptance and verification

- Record real results for arm64 and amd64; a cross-compiled binary alone is not acceptance.
- If a required Mac/runner is unavailable, retain the prompt as blocked rather than substituting Linux results.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/055.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 055 --evidence codex/tracking/evidence/055.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 055 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---

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


---

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


---

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


---

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


---

# 060 — Final handover and CLI release gate

- Phase: 06 Quality and delivery
- Recommended reasoning: **high**
- Depends on: **059**
- Application: Go + Cobra CLI, binary `dockjump`.

## Context and start

Read `AGENTS.md`, `CODEX_START.md`, the architecture and CLI contract, current repository state and earlier evidence. Preserve existing work and reuse completed implementations. Run `python3 codex/scripts/track.py start 060`; resolve unfinished prerequisites if it refuses.

## Goal

Hand over a complete, reproducible CLI project with truthful execution tracking.

## Required implementation

1. Write docs/FINAL_HANDOVER.md with build/test/install commands, command reference, architecture decisions and artifact hashes/locations.
2. Summarize actual native support, known limitations, public distribution readiness and optional future enhancements.
3. Validate prompt statuses/evidence and repository contents without deleting unrelated user work.
4. Complete only when agreed core/native gates are satisfied; otherwise record the exact blocker and next safe action instead of asserting release readiness.

## Acceptance and verification

- Run tracker validate and the final direct/ProxyJump CLI smoke scenario against the tested artifact.
- Ensure output/exit-code documentation matches the actual binary and no credentials/test secrets are included in archives.
- Verify the code path changed here with real results, not a fabricated success message. Distinguish fixtures, live SSH, cross-builds and native runtime evidence.
- Keep stdout/stderr, exit codes, host identity, quoting, resource bounds and permission checks consistent with the shared contract.

## Evidence and finish

Write `codex/tracking/evidence/060.md` using the evidence template. Record changed files, actual commands and results, acceptance decisions, platform and remaining limits. Do not include credentials or production logs.

When acceptance is met:

```bash
python3 codex/scripts/track.py done 060 --evidence codex/tracking/evidence/060.md --check "REPLACE with actual verification and result"
```

Replace the example check; do not submit the placeholder as evidence. If essential work is unavailable or failing, use `python3 codex/scripts/track.py block 060 --reason "Specific failed check or missing prerequisite"`. The reasoning value is advisory and does not change the actual model setting. Stop after this prompt and summarize changes, checks, limitations and the next task.


---
