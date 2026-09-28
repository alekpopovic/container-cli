# Go/Cobra roadmap

| IDs | Milestone |
|---|---|
| 001–008 | Go/Cobra foundation, output contract and local config |
| 009–018 | Native SSH discovery, trust, transport and live checkpoint |
| 019–030 | Container/Compose inspection, logs, stats, events and watch |
| 031–038 | Lifecycle, dry-run, resource views and Compose actions |
| 039–046 | Exec/TTY, signals, groups, completion and diagnostics |
| 047–060 | Tests, CI, archives, native Linux/macOS gates and handover |

All dependencies are intentionally sequential. Missing native infrastructure remains explicit at its acceptance gate. CI is configured before native gates.

| ID | Prompt | Reasoning |
|---|---|---|
| 001 | [Repository audit and CLI contract](../prompts/001-repository-audit-and-cli-contract.md) | high |
| 002 | [Go module and Cobra root command](../prompts/002-go-module-and-cobra-root-command.md) | medium |
| 003 | [Command tree and argument validation](../prompts/003-command-tree-and-argument-validation.md) | medium |
| 004 | [Typed results errors and output contract](../prompts/004-typed-results-errors-and-output-contract.md) | high |
| 005 | [Configuration profiles and precedence](../prompts/005-configuration-profiles-and-precedence.md) | high |
| 006 | [Policy enforcement and confirmation input](../prompts/006-policy-enforcement-and-confirmation-input.md) | high |
| 007 | [Execution interfaces and synthetic fixtures](../prompts/007-execution-interfaces-and-synthetic-fixtures.md) | medium |
| 008 | [Local doctor and deterministic startup](../prompts/008-local-doctor-and-deterministic-startup.md) | medium |
| 009 | [SSH config candidate discovery](../prompts/009-ssh-config-candidate-discovery.md) | high |
| 010 | [Effective SSH resolution and trust boundary](../prompts/010-effective-ssh-resolution-and-trust-boundary.md) | high |
| 011 | [Go OpenSSH process runner](../prompts/011-go-openssh-process-runner.md) | high |
| 012 | [Remote command validation and quoting](../prompts/012-remote-command-validation-and-quoting.md) | high |
| 013 | [Keys host verification and unattended jumps](../prompts/013-keys-host-verification-and-unattended-jumps.md) | high |
| 014 | [Invocation-scoped connection reuse](../prompts/014-invocation-scoped-connection-reuse.md) | high |
| 015 | [Remote Docker capability and endpoint probe](../prompts/015-remote-docker-capability-and-endpoint-probe.md) | high |
| 016 | [Hosts profiles and checks](../prompts/016-hosts-profiles-and-checks.md) | medium |
| 017 | [Structured SSH error classification](../prompts/017-structured-ssh-error-classification.md) | high |
| 018 | [Real SSH vertical slice checkpoint](../prompts/018-real-ssh-vertical-slice-checkpoint.md) | high |
| 019 | [Container inventory adapter](../prompts/019-container-inventory-adapter.md) | high |
| 020 | [Container filtering and stable presentation](../prompts/020-container-filtering-and-stable-presentation.md) | medium |
| 021 | [Container inspect and secret masking](../prompts/021-container-inspect-and-secret-masking.md) | high |
| 022 | [Focused container detail subcommands](../prompts/022-focused-container-detail-subcommands.md) | medium |
| 023 | [Bounded log snapshots](../prompts/023-bounded-log-snapshots.md) | high |
| 024 | [Log follow backpressure and cancellation](../prompts/024-log-follow-backpressure-and-cancellation.md) | high |
| 025 | [Container statistics snapshots](../prompts/025-container-statistics-snapshots.md) | high |
| 026 | [Watch mode for inventory and statistics](../prompts/026-watch-mode-for-inventory-and-statistics.md) | high |
| 027 | [Docker events command](../prompts/027-docker-events-command.md) | high |
| 028 | [Read retries timeout and cancellation policy](../prompts/028-read-retries-timeout-and-cancellation-policy.md) | high |
| 029 | [Compose discovery and service grouping](../prompts/029-compose-discovery-and-service-grouping.md) | high |
| 030 | [Read-only CLI MVP checkpoint](../prompts/030-read-only-cli-mvp-checkpoint.md) | high |
| 031 | [Mutation planning dry-run and activity records](../prompts/031-mutation-planning-dry-run-and-activity-records.md) | high |
| 032 | [Container start stop and restart commands](../prompts/032-container-start-stop-and-restart-commands.md) | high |
| 033 | [Batch outcomes and stopped-container removal](../prompts/033-batch-outcomes-and-stopped-container-removal.md) | high |
| 034 | [Image inventory and inspection commands](../prompts/034-image-inventory-and-inspection-commands.md) | medium |
| 035 | [Volume inventory and references](../prompts/035-volume-inventory-and-references.md) | medium |
| 036 | [Network inventory and attachments](../prompts/036-network-inventory-and-attachments.md) | medium |
| 037 | [Verified Compose project lifecycle actions](../prompts/037-verified-compose-project-lifecycle-actions.md) | high |
| 038 | [Management CLI MVP checkpoint](../prompts/038-management-cli-mvp-checkpoint.md) | high |
| 039 | [Noninteractive container exec passthrough](../prompts/039-noninteractive-container-exec-passthrough.md) | high |
| 040 | [Interactive exec with native terminal behavior](../prompts/040-interactive-exec-with-native-terminal-behavior.md) | high |
| 041 | [Signals process groups and broken pipes](../prompts/041-signals-process-groups-and-broken-pipes.md) | high |
| 042 | [Read-only host groups and fleet snapshots](../prompts/042-read-only-host-groups-and-fleet-snapshots.md) | high |
| 043 | [Shell completion and generated reference docs](../prompts/043-shell-completion-and-generated-reference-docs.md) | medium |
| 044 | [Configuration portability and diagnostics export](../prompts/044-configuration-portability-and-diagnostics-export.md) | medium |
| 045 | [Performance bounds and streaming stress](../prompts/045-performance-bounds-and-streaming-stress.md) | high |
| 046 | [Feature-complete CLI checkpoint](../prompts/046-feature-complete-cli-checkpoint.md) | high |
| 047 | [Focused transport and CLI security review](../prompts/047-focused-transport-and-cli-security-review.md) | high |
| 048 | [Command contract and regression suite](../prompts/048-command-contract-and-regression-suite.md) | high |
| 049 | [Fuzzing parsers and argument encoding](../prompts/049-fuzzing-parsers-and-argument-encoding.md) | high |
| 050 | [Disposable SSH and Docker integration lab](../prompts/050-disposable-ssh-and-docker-integration-lab.md) | high |
| 051 | [Local quality gate and dependency checks](../prompts/051-local-quality-gate-and-dependency-checks.md) | medium |
| 052 | [Linux and macOS CI matrix](../prompts/052-linux-and-macos-ci-matrix.md) | high |
| 053 | [GoReleaser and cross-platform binary archives](../prompts/053-goreleaser-and-cross-platform-binary-archives.md) | high |
| 054 | [Linux native binary acceptance](../prompts/054-linux-native-binary-acceptance.md) | high |
| 055 | [macOS native binary acceptance](../prompts/055-macos-native-binary-acceptance.md) | high |
| 056 | [Installation distribution and optional signing](../prompts/056-installation-distribution-and-optional-signing.md) | medium |
| 057 | [Versioned schemas upgrade and compatibility policy](../prompts/057-versioned-schemas-upgrade-and-compatibility-policy.md) | medium |
| 058 | [Serbian quick start and English runbook](../prompts/058-serbian-quick-start-and-english-runbook.md) | medium |
| 059 | [Release candidate review and final defects](../prompts/059-release-candidate-review-and-final-defects.md) | high |
| 060 | [Final handover and CLI release gate](../prompts/060-final-handover-and-cli-release-gate.md) | high |
