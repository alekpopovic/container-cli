# DockJump command contract v1

This is the implementation specification for the prompt pack. Commands become available as prompts are executed; this document is not proof that a binary already exists. Match generated help and tests to this contract.

## Root flags

| Flag | Meaning |
|---|---|
| `--host ALIAS` | One concrete saved/direct OpenSSH alias |
| `--group NAME` | Saved host group, finite supported reads only; conflicts with --host |
| `--config PATH` | Application JSON config; distinct from SSH config |
| `--ssh-config PATH` | Selected trusted OpenSSH config passed with -F |
| `--ssh-binary PATH` | Validated absolute local OpenSSH executable |
| `--docker-context NAME` | Named context on the remote host, applied consistently |
| `--docker-binary PATH` | Optional validated absolute remote Docker executable |
| `--sudo` | Fixed existing sudo -n Docker mode; no credential prompt |
| `--output auto\|table\|json\|jsonl\|raw` | Command-specific formats listed below |
| `--timeout DURATION` | Finite read/action deadline; default 30s; streams/exec use separate lifetime rules |
| `--connect-timeout DURATION` | Bounded connection/probe setup; default 10s |
| `--write` | Explicit permission for changes or exec in this invocation |
| `--yes` | Skip confirmation after permission/target checks; no implicit write permission |
| `--no-color` | Disable ANSI styling; non-TTY output is always unstyled |

Support only flags meaningful for each command; reject incompatible flags before remote work. Local metadata changes are not remote Docker mutations and do not need --write. Remote write operations require an explicitly provided --host flag, even if reads have a default host.

Ordinary preference precedence: explicit flags > documented DOCKJUMP_* variables > application config > built-in defaults. Initially support DOCKJUMP_HOST, DOCKJUMP_SSH_CONFIG, DOCKJUMP_OUTPUT and DOCKJUMP_TIMEOUT. **Do not support environment/profile defaults for --write or --yes.**

## Command tree

| Command | Important options / behavior |
|---|---|
| `version` | Local version/commit/platform; auto or JSON |
| `doctor local` | Local SSH/config/agent diagnostic; no remote connection |
| `doctor export --file PATH` | Save a redacted local support report |
| `hosts discover` | Literal SSH candidates; no subprocess/network |
| `hosts add ALIAS` | Save optional --label and --group metadata |
| `hosts list` | Saved profiles, local only |
| `hosts show ALIAS` | Local profile; no implicit resolution/probe |
| `hosts remove ALIAS` | Remove app metadata only |
| `hosts use ALIAS` | Default for read operations |
| `hosts check` | Requires selected host; explicit staged SSH/Docker check |
| `hosts group add NAME ALIAS...` | Define/update a local group of saved aliases |
| `hosts group list` | Local group membership |
| `containers ls` | Running by default; --all, --filter, --sort, --quiet, --watch |
| `containers inspect TARGET` | --show-secrets is explicit; masked by default |
| `containers health TARGET` | Lifecycle state and configured health state |
| `containers ports TARGET` | Exposure versus host bindings |
| `containers mounts TARGET` | Mount metadata, no file content access |
| `containers env TARGET` | Values masked unless --show-secrets |
| `containers logs TARGET` | --tail, --timestamps, --since, --until, --follow |
| `containers stats [TARGET...]` | --once default, --watch, --interval, --count |
| `events` | --filter, --since, --until; text or JSONL |
| `containers start TARGET...` | Mutation, --dry-run supported |
| `containers stop TARGET...` | Mutation, --stop-timeout, --dry-run |
| `containers restart TARGET...` | Mutation, --stop-timeout, --dry-run |
| `containers rm TARGET...` | Stopped only; no --force or volume removal |
| `compose ls` | Plugin JSON or label-derived read grouping |
| `compose ps PROJECT` | Read grouping, no local files required |
| `compose start\|stop\|restart PROJECT [SERVICE...]` | Verified --project-dir and repeatable --file; --dry-run |
| `images ls`, `images inspect TARGET` | Read-only image metadata |
| `volumes ls`, `volumes inspect NAME` | Read-only volume metadata |
| `networks ls`, `networks inspect TARGET` | Read-only network metadata |
| `exec [-i] [-t] TARGET -- COMMAND [ARG...]` | Write-capable passthrough; no JSON/table wrapper |
| `completion bash\|zsh\|fish` | Emit shell script; no network |
| `config path`, `config show` | Local paths/preferences, sanitized |
| `config export --file PATH` | Non-secret metadata only |
| `config import --file PATH` | Validate and preview local metadata changes |

For container TARGET, accept a full ID, an exact name or a unique ID prefix resolved on the selected current daemon. Reject ambiguous names/prefixes. Every mutation uses resolved full IDs and rechecks relevant state before dispatch. Do not treat names from another host as interchangeable.

`--filter` supports an explicit documented subset of Docker predicates for that command; validate keys and values. `--sort` initially supports name/state/image for inventory. `--quiet` is ID-only output and conflicts with an explicitly chosen non-auto output format. Watch adds `--interval` (default 5s, bounded positive duration) and optional positive `--count`; it cannot overlap refresh requests.

## Output rules

| Kind | auto | table | json | jsonl | raw |
|---|---|---|---|---|---|
| Finite list/detail/read | Safe table or text | Supported for tabular result | One bounded envelope | One item record per line | Rejected |
| Finite log snapshot | Safe log text | Rejected | Bounded log-record envelope | Log records | Explicit passthrough |
| Logs follow/events/watch | Safe text; optional TTY-only refresh | Only tabular TTY watch | Rejected | Stream records | Logs only |
| Mutation/dry-run | Plan/result text | Results table | Result envelope | Item outcomes | Rejected |
| exec | Native stream passthrough | Rejected | Rejected | Rejected | Native stream passthrough |

Help, completion generation and docs-generation output have their native text formats. Errors use stderr. In json/jsonl modes errors on stderr are JSON, one complete error record per diagnostic. Never print spinners, progress lines or usage into result stdout. Shell scripts should check exit status, not infer success merely from a syntactically valid result.

For finite reads that fail before a successful result, stdout is empty. A batch/fleet operation may produce a well-formed complete envelope with both results and item errors and return nonzero. A JSONL stream can emit records and later fail; already emitted lines remain valid, and the final error goes to stderr with a nonzero exit.

## Finite v1 JSON shape

Example fixture, not a measured server result:

```json
{
  "schemaVersion": "v1",
  "command": "containers.ls",
  "host": "app-server",
  "items": [
    {
      "host": "app-server",
      "id": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
      "name": "api",
      "state": "running",
      "status": "Up 2 hours",
      "image": "example/api:dev"
    }
  ],
  "errors": []
}
```

Always emit arrays for items/errors, including empty arrays. A group query sets envelope host to null and tags every item/error with its concrete host. Keep stable types and add fields compatibly. Do not emit raw inspect environment secrets inside this wrapper by default. Local commands can set host to null.

## Stream record shape

```json
{"schemaVersion":"v1","type":"log","host":"app-server","observedAt":"2026-09-28T17:00:00Z","data":{"channel":"ssh_stdout","text":"server ready"}}
```

Use type values `item`, `log`, `event`, `snapshot`, `gap` and `truncated` with command-specific typed data. observedAt is the local observation time; retain a remote timestamp separately if available. `ssh_stderr` is channel provenance, not proof that the line came from the application: SSH and Docker diagnostics can share that channel. Preserve text as encoded data; neutralize control sequences only for safe human rendering. The explicitly requested raw mode preserves bytes and their terminal effects.

## Error and exit code contract

```json
{"schemaVersion":"v1","error":{"code":"SSH_CONNECT_FAILED","host":"app-server","message":"SSH connection failed","retryable":true}}
```

| Exit | Meaning for DockJump-managed commands |
|---|---|
| 0 | Successful command or clean finite stream completion |
| 1 | Internal/unexpected failure |
| 2 | Invalid command, arguments or flag combination |
| 3 | Local config/dependency problem |
| 4 | SSH/transport/authentication/trust failure |
| 5 | Policy denied or required confirmation unavailable/declined |
| 6 | Remote Docker operation/unsupported capability failure |
| 7 | Partial batch/fleet failure with known outcomes |
| 8 | At least one dispatched mutation has unknown outcome |
| 124 | Wrapper deadline exceeded before a known outcome; mutation-after-dispatch uses 8 |
| 130 | Wrapper interrupted with SIGINT, unless a write outcome is unknown |
| 141 | Data pipe closed; perform cleanup before returning |
| 143 | Wrapper terminated with SIGTERM, unless a write outcome is unknown |

**exec is a deliberate exception:** once running, it returns the SSH-reported child status when available. That can coincide numerically with wrapper codes; explain origin in diagnostics when needed. SSH 255 is ambiguous with a remote program returning 255 and cannot be reliably classified from the number alone. Setup errors before exec dispatch use wrapper codes. Native PTY behavior may merge remote stderr/stdout. A killed SSH client cannot always reveal the eventual remote program status.

For ordinary operations, if a mutation was dispatched and its outcome is unknown, use code 8 ahead of generic timeout/signal/partial status. Cancellation before dispatch uses the normal wrapper cancellation code. Interactive exec is not automatically retried and follows its documented status exception.

## Permission and input rules

Remote reads can use --host, a default host or an explicit supported --group. Remote mutations/exec require explicit --host and --write; a group is rejected. --yes only replaces the final confirmation. --dry-run is read-only execution of a proposed mutation plan, still requiring explicit host/write intent, and does not require a final execution confirmation.

Read confirmations from /dev/tty, never from a data stdin pipe. Without an interactive terminal, require --yes before execution. Do not infer consent from empty input/timeouts. Local metadata imports confirm replacement in the same input-safe way or use an explicit local --yes.

For `exec`, require exactly one target before `--`, and at least one command token after it. All post-separator flags belong to the container command. `-i` enables stdin forwarding; `-t` requests a terminal and requires a real local TTY. Without -i close child stdin rather than keeping a pipe open. -it is the standard shell use. `--output json` is rejected for exec.

The default snapshot --timeout does not truncate log follow, watch, events or exec. They use connection/setup bounds and cancellation. A future explicit stream/exec lifetime limit requires a separately documented option; do not silently reuse the snapshot default.
