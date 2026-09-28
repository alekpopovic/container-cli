# DockJump platform verification — copy to docs/platform-matrix.md

All fields begin as **NOT RUN**. Build success and runtime success are separate. No application/native result is supplied by the prompt package itself.

| Check | Linux amd64 | Linux arm64 | macOS amd64 | macOS arm64 |
|---|---|---|---|---|
| OS/version/architecture/runner | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| App commit / Go / OpenSSH / Docker versions | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Cross/native build, artifact hash | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Archive install / help / version | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Direct SSH and strict known_hosts | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| ProxyJump to private target | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Agent with encrypted identity | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Human table / JSON / JSONL output | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Log follow / watch / bounded buffers | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Mutation / partial / unknown outcome | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Compose paths / service actions | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Exec stdin and returned status | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Exec -it / resize / Ctrl-C / restore | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| SIGINT / SIGTERM / EPIPE cleanup | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Bash / zsh / fish completion as available | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| User SSH master remains intact | NOT RUN | NOT RUN | NOT RUN | NOT RUN |
| Public signing/notarization | Optional policy | Optional policy | UNVERIFIED | UNVERIFIED |

Each completed field links to exact commands/results, sanitized logs and checksums. If a runner uses emulation, label that fact explicitly. Do not present an emulated or cross-built artifact as proof of native execution on physical target hardware. Missing required platform evidence remains a blocker at its acceptance prompt.
