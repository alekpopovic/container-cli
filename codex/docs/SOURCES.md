# Primary references

Reviewed 2026-09-28. The task ordering, component structure, output schemas and proposed limits in this package are engineering design choices. Select compatible exact versions during prompt 001; upstream documentation can change independently of pinned project dependencies.

| Topic | Primary source | Application |
|---|---|---|
| Cobra | https://github.com/spf13/cobra/blob/main/site/content/user_guide.md | Command tree, flag/argument handling and generated help |
| Cobra flags | https://cobra.dev/docs/how-to-guides/working-with-flags/ | Flag applicability and validation |
| Cobra completion | https://cobra.dev/docs/how-to-guides/shell-completion/ | Shell completion generation/hooks |
| Go subprocesses | https://pkg.go.dev/os/exec | Native process execution, cancellation and I/O behavior |
| Go terminal APIs | https://pkg.go.dev/golang.org/x/term | TTY detection and terminal state if needed |
| OpenSSH client | https://man.openbsd.org/ssh.1 | Remote execution, PTY, control and exit semantics |
| OpenSSH config | https://man.openbsd.org/ssh_config.5 | Existing aliases, jump hosts, keys, includes and executable config |
| GoReleaser Go builds | https://goreleaser.com/customization/builds/builders/go/ | Target build matrices and archive preparation |
| Docker inventory | https://docs.docker.com/reference/cli/docker/container/ls/ | JSON-template container listing |
| Docker logs | https://docs.docker.com/reference/cli/docker/container/logs/ | Snapshot and follow options |
| Docker stats | https://docs.docker.com/reference/cli/docker/container/stats/ | CLI snapshots and metric semantics |
| Docker exec | https://docs.docker.com/reference/cli/docker/container/exec/ | Container argv, stdin and terminal options |
| Docker events | https://docs.docker.com/reference/cli/docker/system/events/ | Events/filter stream |
| Docker Compose list | https://docs.docker.com/reference/cli/docker/compose/ls/ | JSON project inventory |
| Docker SSH access | https://docs.docker.com/engine/security/protect-access/ | SSH daemon access and remote-user permissions |

Use the official docs for the locked versions during implementation. Avoid copying a GoReleaser example for a different major schema or assuming command cancellation automatically cleans every descendant process.
