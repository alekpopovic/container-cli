# Start or resume DockJump

Run commands from the repository root. Read `AGENTS.md`, `README_SR.md`, `codex/docs/ARCHITECTURE.md`, `codex/docs/CLI_CONTRACT.md` and `codex/docs/TRACKING.md`.

## Paste into Codex

```text
Implement the next DockJump Go/Cobra CLI task in this repository.
Read AGENTS.md and CODEX_START.md, then run:
python3 codex/scripts/track.py validate
python3 codex/scripts/track.py next

Read the exact returned prompt and the architecture/CLI contract.
Inspect current code, Git changes and evidence before editing.
Run tracker start for the selected task. Implement one concrete increment,
run meaningful checks, write actual evidence and mark done or blocked.
Keep the app in Go with Cobra and native OpenSSH; follow the documented
stdout/stderr, JSON, exit code, host selection and exec argument contracts.
Use the recommended reasoning if your interface supports it; otherwise
state the recommendation without claiming a model setting was changed.
Do not execute a second prompt automatically. Return changes, real check
results, limitations and the next task. Preserve unrelated user work.
```

## Continuing

Use the same message in a fresh session. `next` returns the earliest unfinished task, including active/blocked work. Resolve the blocker and run `start NNN` to resume. Do not replace progressed state with the initial ZIP state.

The pack begins with zero completed application tasks. Record evidence in `codex/tracking/evidence/NNN.md`; fixture checks, cross-builds and actual native runtime checks are different results. Prompt 018 requires a real disposable SSH target, and native acceptance later requires Linux/macOS execution evidence.

The Python script only tracks prompt work. The production application is a Go binary and does not require Python. No special Codex model name or undocumented command-line flag is required by this pack.
