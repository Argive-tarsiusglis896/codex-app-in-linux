# Codex app Linux-wrapper mac-parity QA — 2026-06-20

Status: IN PROGRESS — Lane A seeded the matrix, oracle taxonomy, gate ledger, and report skeleton. Lane B/C evidence and final review are pending.

## Run artifacts

- Manifest: `evidence/mac-parity-qa-20260620/run-manifest.md`
- Taxonomy: `evidence/mac-parity-qa-20260620/taxonomy.md`
- Gates: `evidence/mac-parity-qa-20260620/gates.md`
- Matrix CSV: `evidence/mac-parity-qa-20260620/feature-matrix.csv`
- Matrix MD: `evidence/mac-parity-qa-20260620/feature-matrix.md`
- Lane index: `evidence/mac-parity-qa-20260620/lane-evidence-index.md`
- Verdict skeleton: `evidence/mac-parity-qa-20260620/final-verdict.md`

## Scope and boundary

Official Codex app support is macOS and Windows. This report evaluates unofficial Linux wrapper compatibility only and must not be quoted as OpenAI Linux desktop support.

## Seeded facts from current repo evidence

- Wrapper commit: `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`
- Latest observed upstream app version: `26.616.41845`
- Electron version: `42.1.0`
- Existing local proof: PASS WITH LIMITATIONS.
- Existing PASS seeds: startup/window/render, existing account readiness, basic chat, harmless shell command, chat search, Browser Use on `https://example.com`.
- Existing PARTIAL seeds: project/file dialog, Chrome native-host configured on disk but not live-connected.
- Existing NOT SUPPORTED seed: Computer Use disabled/not exposed on Linux wrapper; no helper install attempted.
- Existing UNTESTED seeds: fresh login, settings restart persistence, full project workflow, worktrees, Git review/stage/commit/push/PR, automations, MCP/plugins, Sites, IDE sync, web search, image input/generation, memories, notifications, prevent sleep, remote control, Appshots.
- Security seed: Electron launches with `--no-sandbox`; webview evidence must prove loopback binding.

## Gate status

No global denied gate is approved in this run. `sudo`, `pkexec`, package-manager commands, `systemctl`, native install/updater/service/package work, personal browser profiles, production remote push/PR, sensitive signed-in websites, Computer Use helper install, source edits, commits, and pushes remain denied.

## Current matrix summary

Lane A seeded 38 feature/security rows across startup/account/project/chat, terminal/actions, Git/worktrees/review, browser surfaces, Chrome extension, Computer Use/Appshots, integrations, automations, settings, desktop notifications/prevent-sleep, remote control, and wrapper security rows.

Final counts are pending Lane B and Lane C updates.

## Lane B findings

Pending.

## Lane C findings

Pending.

## Final review

Pending final architect and critic review before any parity declaration.

## Recommendation

Pending. Current safe interim recommendation remains: use generated local wrapper only for non-system local GUI proof workflows already covered by evidence; keep official Codex app on macOS/Windows or Codex CLI/SSH as the supported fallback.
