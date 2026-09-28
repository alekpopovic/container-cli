# Repository rules — DockJump

Build one Go CLI with Cobra, native OpenSSH and the remote Docker CLI. Deliver Linux/macOS terminal workflows and binary archives. Read the architecture and CLI contract before implementing the selected prompt.

## Execution

- Inspect existing code and uncommitted work. Do not overwrite unrelated files or initialize a nested application.
- Use `python3 codex/scripts/track.py next`, then execute only that prompt with `start`/`done`/`block` and real evidence.
- Preserve immutable prompt files and hashes; record approved design changes in decisions/evidence.
- Pin supported Go/Cobra/dependency versions in the first steps. Use small dependency-injected services and fresh Cobra command trees.
- Reasoning fields are recommendations, not executable model settings.
- Do not fabricate live SSH/native platform checks or call a cross-build a runtime test. Keep missing required gates blocked.

## Command contract

- Follow `codex/docs/CLI_CONTRACT.md` as the authoritative proposed command grammar and output protocol.
- stdout contains result data; stderr contains diagnostics/prompts. Finite JSON is emitted atomically after a successful bounded read.
- Streaming JSONL must remain one object per line. Do not mix progress text, color or Cobra usage into it.
- Validate flags and positional arguments before opening SSH. Help/version/completion generation are local.
- Keep cleanup in a top-level run function; only main converts the final result to os.Exit after cleanup.
- exec uses an explicit `--` boundary and forwards the user-provided container argv. Do not parse it as a host-side shell script.

## SSH, resources and permissions

- Spawn native SSH with os/exec argument slices, not a local shell. Validate inputs and quote every remote argument centrally for a POSIX remote shell.
- Let OpenSSH implement effective config/jump/identity semantics. Discover literal aliases without executing config; selected trusted config may contain executable directives.
- Preserve strict known_hosts checking and use existing keys/agent. Never silently edit SSH config/trust, enable agent forwarding or collect passwords.
- Own control sockets/processes per invocation; never terminate another program's master. Use bounded timeouts, queues and process cleanup.
- Enforce --host plus --write for mutations/exec in the service layer. --yes skips confirmation only; it does not grant permission.
- Never consume piped stdin for a confirmation. Use /dev/tty when available or require --yes.
- No automatic retry of mutation/exec after dispatch. A disconnected result may be unknown; read current state to reconcile.
- Mask sensitive inspect metadata before output; no keys, passwords, logs or exec input in default diagnostics/persistent config.

## Scope and testing

Keep one binary, no resident daemon or remote agent. Read views include containers/Compose/images/volumes/networks; writes are lifecycle operations, explicit stopped-container removal without force/volume deletion, verified existing Compose lifecycle and container exec. Prune, deployment/build, registry credential management, Kubernetes and a full-screen TUI are outside v1.

Use disposable SSH/Docker test resources rather than guessing a production alias. Write meaningful tests for quoting/parsing, output/exit contracts, policy, cancellation and native terminal handling. An app read-only rule does not remove privileges from the remote SSH/Docker account.

Build and prepare release artifacts locally or through configured CI. Public publishing, accounts and signing credentials are separate owner-controlled actions unless explicitly authorized. Never claim signing or a native architecture is verified without real evidence.
