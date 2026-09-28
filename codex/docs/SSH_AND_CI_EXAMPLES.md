# SSH and scripting examples

These examples describe the intended CLI after implementation. The SSH aliases and container names are placeholders for the user's own configured test resources.

## Existing terminal baseline

```bash
ssh app-server 'docker ps -a --no-trunc --format "{{json .}}"'
```

DockJump uses the same alias with native OpenSSH. It encodes remote argv in Go and parses the Docker JSON itself. It does not need local Docker or jq. The optional jq examples below process DockJump output.

## Read examples

```bash
dockjump hosts discover
dockjump hosts add app-server --label "Application server" --group staging
dockjump --host app-server hosts check
dockjump --host app-server containers ls --all
dockjump --host app-server containers ports api
dockjump --host app-server containers logs api --tail 100 --follow --output jsonl
dockjump --host app-server containers stats --watch --interval 5s --count 12 --output jsonl
```

## Shell pipeline

```bash
# Bash example: fail the pipeline when dockjump fails, even if jq exits zero.
set -o pipefail
dockjump --host app-server containers ls --all --output json |
  jq -r '.items[] | [.host, .name, .state, .image] | @tsv'
```

`pipefail` is a shell setting, not a flag implemented by DockJump. A live JSONL stream may emit valid records and later fail; check its process exit status too. `head` closing a data pipe can deliberately end a producer with the documented 141 code after cleanup.

## Scripted mutations

```bash
# Plan first; no remote mutation is dispatched.
dockjump --host app-server --write containers restart api --dry-run --output json

# Intent and confirmation are explicit for unattended execution.
dockjump --host app-server --write --yes containers restart api --output json
```

If the result is unknown (code 8), read current state before deciding whether another action is needed:

```bash
dockjump --host app-server containers inspect api --output json
```

Do not wrap a restart command in a generic automatic retry loop; the first restart may have succeeded remotely.

## Container exec

```bash
dockjump --host app-server --write --yes exec -it api -- /bin/sh

# Read stdin inside the container; confirmation must not consume input.txt.
dockjump --host app-server --write --yes exec -i api -- cat < input.txt

# Args after -- belong to the program inside the container.
dockjump --host app-server --write --yes exec api -- /bin/sh -c 'printf "%s\n" "hello"'
```

The existence of /bin/sh or cat depends on the selected container image. exec returns the SSH-reported remote status when available; 255 can also represent an SSH transport failure. It is deliberately not JSON-wrapped. For an interactive terminal, keep native terminal mode/resize handling with OpenSSH rather than parsing its byte stream as structured output.

## Existing remote Compose project

```bash
dockjump --host app-server compose ls
dockjump --host app-server compose ps myapp
dockjump --host app-server --write --yes compose restart myapp api \
  --project-dir /srv/myapp --file /srv/myapp/compose.yaml
```

The paths are remote and must actually exist/read correctly for the chosen SSH user and privilege mode. Labels alone do not grant a trustworthy runnable configuration. This release does not create/deploy/build a project.

## Completion

Generate scripts and install them using the instructions for the chosen shell. Generation must not contact servers:

```bash
dockjump completion bash > dockjump.bash
dockjump completion zsh > _dockjump
dockjump completion fish > dockjump.fish
```

## Development verification targets

The prompt sequence creates actual Makefile/CI targets for Go formatting, vet, test/race, build, bounded fuzzing and optional integration. Do not assume those targets exist in the initial prompt ZIP. The development tracker is already runnable:

```bash
python3 codex/scripts/track.py next
python3 -m unittest discover -s codex/tests -v
```
