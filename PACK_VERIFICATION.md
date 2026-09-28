# Verification of the Go/Cobra prompt package

Date: 2026-09-28

## Checks performed

- Python 3.12.14 on the Linux preparation environment.
- `python3 -m unittest discover -s codex/tests -v`: **14 tests passed** in the new CLI package.
- Tracker validation checked prompt IDs, dependency ordering, file hashes and the clean initial state.
- The package contains **60 numbered Go/Cobra implementation prompts**, reproduced verbatim in `codex/ALL_PROMPTS.md`.
- Prompt sections, relative Markdown links and the illustrative JSON output were checked.
- Initial execution status is **60 pending**, with no active/completed/blocked tasks and no fabricated application evidence.

Tracker coverage includes prerequisites, one active task, block/resume, completion evidence, outside-project/symlink rejection, tampered prompt/evidence hashes, cascading reopen, concurrent writes and Markdown rendering. Tests run on isolated temporary copies without SSH or network access.

## Integrity

`SHA256SUMS.txt` lists the distributed file hashes. Tracker state and evidence normally change as prompts are executed, so those files will subsequently differ. The immutable prompt hashes remain in `codex/manifest.json`.

## Scope of verification

This ZIP is a prompt-and-tracker deliverable. It does not contain an implemented or tested DockJump binary. The Go application, real SSH/Docker connections, native terminal behavior, Linux/macOS runtime, GoReleaser artifacts and signing are implemented/verified during the prompt sequence.

The tracker was executed on Linux; its macOS use relies on the corresponding Python/POSIX APIs but was not independently run on macOS while producing this pack. Future completion evidence must distinguish fixture tests, cross-builds and native runtime checks.

The CLI architecture and commands were rewritten for Go/Cobra. Generic tracker utilities were reused from the earlier package and re-tested. The earlier desktop package was not modified.
