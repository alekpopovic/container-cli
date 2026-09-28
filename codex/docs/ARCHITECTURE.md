# DockJump architecture contract

Proposed implementation: one Go/Cobra CLI using native OpenSSH to run Docker commands on Linux servers. This package describes work to implement; it does not contain an already implemented application. Exact supported versions are selected in prompt 001 and locked in go.mod/go.sum.

## Flow

A Cobra command validates input, resolves local preferences and builds a typed operation. A Go service checks policy and target selection. A transport adapter runs the installed OpenSSH executable using the original alias and chosen SSH config. OpenSSH applies ProxyJump/identity/known_hosts rules and runs a centrally quoted remote Docker command. Go decodes bounded output, renders results and returns a documented status after all owned resources are cleaned up.

Every CLI invocation is self-contained. Saved host profiles are local preferences, not live sessions. A watch/fleet command may reuse its own SSH master for its lifetime. There is no background daemon, remote agent, web backend or GUI runtime.

## Implementation choices

| Area | Choice |
|---|---|
| Language | Go, supported stable toolchain selected at start |
| Command framework | github.com/spf13/cobra and its pflag integration |
| Encoding/config | encoding/json and explicit precedence |
| Human tables | Standard library tabwriter or a small measured formatter |
| Transport | os/exec invoking native OpenSSH |
| Cancellation | context plus explicit Unix child/group ownership |
| Signals | os/signal with distinct stream versus interactive-exec paths |
| Terminal detection | golang.org/x/term or equivalent supported native APIs |
| Interactive session | Inherited terminal descriptors, native SSH owns PTY behavior |
| Release tooling | GoReleaser, tested current schema, local snapshots first |
| Prompt tracker | Python standard library; development-only utility |

Avoid a second SSH implementation merely to reduce the number of external executables. golang.org/x/crypto/ssh alone does not provide the full behavior of the user's installed OpenSSH config. Native ssh is an intentional runtime dependency. Local Docker and jq are optional external convenience tools, not app dependencies.

## Proposed layout

| Path | Responsibility |
|---|---|
| `cmd/dockjump/main.go` | Minimal main, final process exit only |
| `internal/cli/` | Fresh Cobra constructors and command adapters |
| `internal/config/` | JSON profiles, precedence, migration and locking |
| `internal/sshconfig/` | Nonexecuting candidate discovery and selected resolution |
| `internal/transport/` | Exec, bounded I/O, sessions, signals and ownership |
| `internal/docker/` | Typed builders and output decoders |
| `internal/service/` | Operations, policy and host/daemon binding |
| `internal/output/` | Safe tables, JSON envelopes and JSONL records |
| `internal/terminal/` | TTY checks and interactive-exec lifecycle |
| `internal/testutil/` | Inert child-process fixtures, clocks and fake adapters |
| `testdata/` | Synthetic non-secret parser and output fixtures |
| `tests/integration/` | Opt-in disposable SSH/Docker lab |
| `docs/` | Actual decisions, runbook and verification evidence |
| `codex/` | Prompts and execution tracking |

Adapt existing directories where appropriate rather than creating duplicate implementations. Pass dependencies into constructors; avoid global mutable flags, init-time I/O and expensive package initialization.

## SSH configuration

Discovery parses candidate names without evaluating the configuration. Literal Host entries can be presented; wildcard/negative patterns cannot enumerate all possible hosts. Include processing needs bounds, cycle handling, comments/quoted path support and clear partial-discovery feedback. A user may manually choose a concrete alias.

Effective configuration is resolved by the installed OpenSSH after explicit selection. The same executable, alias and -F path must be used for diagnostics and real operations. `ssh -G` can evaluate `Match exec`: trusted user configuration is an executable local trust boundary, not an inert imported data file. Completion must not run this resolver or contact a server.

Use a documented simple alias grammar, for example an ASCII alphanumeric first character followed by alphanumerics/dot/underscore/dash with a finite length. Reject leading dashes and control/whitespace. Recommend a simple SSH alias for destinations requiring more complex syntax.

Use the OS SSH agent and configured identities with strict host verification. Bootstrap trust and encrypted keys in the user's terminal through independently checked fingerprints. No automatic trust acceptance, disabling checks, agent forwarding, credential storage or silent config edits. A ProxyJump child has its own effective host rules; test both hops instead of assuming destination options cover it.

## Process ownership and cancellation

Build `exec.Cmd` from a validated executable and individual arguments. Do not invoke a local shell for structured operations. Use separate bounded stdout/stderr readers and ensure neither pipe blocks the other. A deadline or writer error cancels/reaps owned processes. Pay particular attention to children holding pipes open after the parent exits; configure a finite drain policy using APIs supported by the pinned Go version.

For noninteractive operations, explicitly manage a Unix process group as needed to clean owned descendants. Killing only the initial process via CommandContext may be insufficient. Do not signal arbitrary shared process groups. Interactive exec inherits the terminal and needs a separate foreground/signal strategy; test it using a PTY harness.

Per-invocation masters use unique private short Unix-socket paths, preventing collisions across parallel CLI invocations. Never close a user's pre-existing SSH master. App-created master cleanup is bounded and scoped to its owner. Avoid keeping ControlPersist masters indefinitely after the command returns.

## Remote shell and argv

OpenSSH sends a remote command that passes through the remote shell. Local argument arrays alone are not protection at that boundary. For supported POSIX-compatible remote login shells, quote each token with a single central POSIX encoder and validate values before encoding.

Structured operations use typed enums/builders for fixed Docker commands, validated flags/IDs and bounded numbers. Human display names, labels and output values never become host-side shell fragments. Optional remote executable/project paths are treated as data, never evaluated.

`exec TARGET -- COMMAND ARG...` deliberately executes the user's argv inside the selected container and is treated as write-capable. Tokens after -- are quoted individually. If the user explicitly runs `/bin/sh -c ...`, that script is a command inside the container, not a reason to interpolate it into the host transport command unsafely. There is no generic host-script runner.

## Docker endpoint and privilege mode

The remote user's default Docker context/endpoint is the default. A named remote context can be explicitly selected, then applied to every read and write. Show SSH host and actual Docker daemon identity separately because a remote context can itself point elsewhere. Cache/operation identity includes this endpoint.

Support rootless sockets through the selected remote context/environment and an optional explicit fixed `sudo -n` mode for an already configured policy. The app does not install Docker or alter Unix group/sudo permissions. Reject interactive sudo requirements with a clear setup error. The supported remote daemon OS is Linux.

## Output and errors

The exact grammar, JSON shapes and codes live in CLI_CONTRACT.md. All handlers return typed errors; the top level prints one coherent diagnostic and decides the process status. Finite JSON is bounded and emitted after successful parsing. Partial batch/group results are complete envelopes with item errors and nonzero status. Streaming output may end after valid earlier records with a diagnostic/nonzero status.

Do not blindly forward arbitrary Docker stderr as an application error: container logs may contain legitimate application stderr. In log records use the actual SSH channel name. Exact attribution is not possible from this channel alone. Human display sanitizes terminal escapes; explicit raw output preserves bytes. Treat missing values as unknown, not zero.

## Permissions and mutations

Service methods enforce explicit --host plus --write for lifecycle, removal, Compose changes and exec. --yes only bypasses a prompt. Interactive confirmation uses /dev/tty so input intended for a container is never consumed. Dry-run builds a plan with reads and no mutation dispatch. No persisted/environment permission enables writes automatically.

Recheck exact target IDs and daemon identity before changes. Finite batches report individual outcomes and partial success. Do not automatically retry a dispatched write or exec command: a lost response may mean it succeeded. Unknown mutation outcomes take precedence over generic timeout/cancellation codes, with a suggested read reconciliation command.

This application permission layer does not constrain what another program can do with the same SSH/Docker account. Local activity history is optional, finite and sanitized, not a tamper-proof audit trail.

## Streaming and terminal behavior

Use read snapshots for inventory/statistics and JSONL for live streams/watch. Keep only bounded queue/buffer state and enforce line sizes explicitly rather than accepting bufio.Scanner's default cap accidentally. Watch is a modest polling loop, not a full-screen TUI. On non-TTY outputs never emit screen-clear/cursor codes.

For interactive exec, attach inherited terminal descriptors directly to SSH and request a remote PTY; native SSH performs local terminal behavior. Use the command's own -i/-t semantics consistently. Do not detach the controlling terminal or wrap the path in an unnecessary second emulator. Test Ctrl-C, SIGWINCH/resize, shell exit and abrupt connection loss, including restored terminal state.

The global finite timeout must not terminate long-lived log/watch/event/exec sessions after 30 seconds. Their setup is bounded; lifetime ends through EOF, explicit count/until, consumer failure or cancellation. Reconnection for read streams is optional and exposes gaps. Exec input is never replayed.

## Proposed default limits

| Resource | Initial design bound |
|---|---|
| SSH setup | 10s connection setup, bounded complete attempt |
| Finite operation | 30s deadline unless explicitly adjusted |
| Captured finite output | 16 MiB, then explicit too-large error |
| Diagnostic stderr | 256 KiB rolling buffer before redaction |
| Log record | 256 KiB, explicit truncation or bounded raw passthrough |
| Stream queue | 8 MiB maximum retained data, explicit pressure policy |
| Watch interval | 5s default; minimum 1s |
| Host group concurrency | 4 simultaneous hosts maximum by default |
| Mutation target batch | 50 maximum, 2 dispatched concurrently by default |
| Connection config include | Finite depth/total size established in discovery tests |

These are proposed implementation defaults to measure and adjust with documented evidence, not achieved benchmark claims. Raw exec is byte-streamed without retaining its full output.

## Native build and release

Create linux/amd64, linux/arm64, darwin/amd64 and darwin/arm64 archives, preferably CGO_ENABLED=0 if chosen dependencies support it. Use GoReleaser snapshot builds before release; pin compatible schema/tool versions. Cross-compilation proves a build exists, while native runtime verification requires the target environment.

Native acceptance should record every advertised verified target. Keep untested architecture results explicit instead of assuming an amd64 build proves arm64 behavior. A release scope change must be an explicit documented decision rather than quietly deleting an agreed target. macOS signing/notarization is a separate public distribution field; missing credentials must not be mislabeled as verified.

CI is configured before native acceptance gates so remote runners can provide the needed environments. Publishing, package registries, accounts, updates and telemetry are not automatic parts of this initial implementation.
