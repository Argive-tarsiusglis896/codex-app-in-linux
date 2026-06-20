# Lane B — Linux local app core QA

Date: 2026-06-20 UTC  
Worker: worker-2  
Task: `task-2` / Lane B

## Verdict

PASS WITH LIMITATIONS for safe local Linux generated-app QA. Fresh startup checks passed: local webview returned HTTP 200, the current launcher log shows webview origin verification, Electron spawn, stdio app-server transport, Codex CLI initialization, primary window ready-to-show, repeated `account/read` success, Browser Use IAB backend ready, and updater disabled. Existing just-now interactive smoke evidence was copied into this run directory and shows controlled chat, harmless shell command, chat search, and Browser Use success.

Limitations: full DOM interaction through a plain headless Chromium tab is not representative because the app depends on Electron host IPC; project file dialog and settings restart were not retested to avoid unsafe/mutating UI paths; disposable Git/review UI was observed only through loaded app assets/process evidence, not through an end-to-end PR flow.

## Run-specific artifacts

- `evidence/mac-parity-qa-20260620/lane-b/launch-status.json`
- `evidence/mac-parity-qa-20260620/lane-b/http-smoke.json`
- `evidence/mac-parity-qa-20260620/lane-b/current-launcher-port-5175.log`
- `evidence/mac-parity-qa-20260620/lane-b/current-launcher-signals.json`
- `evidence/mac-parity-qa-20260620/lane-b/process-snapshot.json`
- `evidence/mac-parity-qa-20260620/lane-b/prior-chat-feature-smoke.json`
- `evidence/mac-parity-qa-20260620/lane-b/prior-interactive-qa-summary.json`
- `evidence/mac-parity-qa-20260620/lane-b/prior-webview-smoke-result.txt`
- `evidence/mac-parity-qa-20260620/lane-b/lane-b-summary.json`

## Checks

| Surface | Result | Evidence |
| --- | --- | --- |
| Startup / webview | PASS | `http-smoke.json`: HTTP 200; `current-launcher-signals.json`: `webview_origin_verified=true`, `electron_spawned=true`. |
| Version/runtime readiness | PASS | Current launcher log and process snapshot show generated app `26.616.41845`, Electron `42.1.0`, managed Node runtime, Codex CLI `0.136.0`. |
| App-server transport | PASS | Current launcher log: `Starting app-server connection hostId=local transport=stdio`, `Codex CLI initialized`. |
| Account readiness | PASS | Current launcher log contains repeated successful `method=account/read` responses with `errorCode=null`. |
| Basic chat | PASS (copied prior smoke) | `prior-chat-feature-smoke.json`: `basicChatSucceeded=true`, observed `codex-linux-qa-ok`, no `inputSchema` error. |
| Multi-chat/search | PARTIAL/PASS | Search smoke passed in `prior-chat-feature-smoke.json`; thread/list routes loaded in current launcher log. Multi-chat was not independently expanded beyond existing thread list/current smoke. |
| Project/file dialog | PARTIAL | `prior-interactive-qa-summary.json`: project menu opens and offers Start from scratch / Use existing folder; native file dialog selection not exercised. |
| Harmless terminal/actions approval | PASS (copied prior smoke) | `prior-chat-feature-smoke.json`: shell command succeeded and observed `codex-linux-shell-ok`. |
| Settings restart | NOT RETESTED | No safe settings restart mutation performed in this lane. |
| In-app Browser Use | PASS | Current launcher log: `browser_use_iab_backend_startup_ready`; prior smoke observed `Example Domain`. |
| Disposable Git/worktree/review UI | PARTIAL | Disposable project path launched; current assets include git/worktree/review UI modules; no production remote/PR operation performed. |
| Plain browser automation | NOT A PRODUCT FAILURE | Headless Chromium loaded title `Codex` from localhost but only root shell elements because Electron host IPC is required. |

## Gates observed

- No sudo, pkexec, package-manager install, systemctl/service mutation, native install, updater, package build/install, push, PR, or Computer Use helper install was run by this lane.
- Browser automation used the tool default ephemeral browser against `127.0.0.1`; no personal browser profile was used.
- Sensitive signed-in websites were not opened.

## Handoff for final matrix

Lane B should be classified as `pass-with-limitations` for local Linux core operation: startup, renderer/server, app-server handshake, account readiness, chat, shell approval smoke, search, and Browser Use smoke pass. Keep project file dialog, settings restart, multi-chat depth, and Git/review UI as partial/not-retested rows unless another lane has stronger direct UI evidence.
