# Lane evidence index

## Lane A — matrix, oracle, evidence marshal

| Artifact | Purpose | Status |
| --- | --- | --- |
| `run-manifest.md` | Run pins, source oracle set, gates summary | Seeded |
| `taxonomy.md` | Verdict and evidence classification rules | Seeded |
| `feature-matrix.csv` | Machine-readable canonical matrix | Seeded |
| `feature-matrix.md` | Human-readable canonical matrix | Seeded |
| `gates.md` | Denylist and conditional gate ledger | Seeded |
| `final-verdict.md` | Final verdict skeleton | Seeded |

## Lane B — Linux local app core QA handoff slots

Expected artifacts should be placed under `evidence/mac-parity-qa-20260620/linux-candidate/` or referenced here:

| Scenario cluster | Expected proof | Status |
| --- | --- | --- |
| Startup/version/account readiness | launch log, version/about evidence, account readiness transcript, screenshot if possible | Pending Lane B |
| Basic chat / multi-chat / search | transcript/logs showing synthetic prompts and search result | Pending Lane B |
| Project/file dialog | disposable directory path, UI/log evidence, cleanup proof | Pending Lane B |
| Terminal/actions approval | harmless command transcript and deny/cancel behavior where safe | Pending Lane B |
| Settings restart | before/after setting, restart log, restoration evidence | Pending Lane B |
| In-app Browser Use | public/local non-auth page transcript/screenshot | Pending Lane B |
| Disposable Git/worktree/review UI | fixture repo state, worktree/diff/stage/commit evidence, cleanup | Pending Lane B |

## Lane C — integrations, security, final review handoff slots

Expected artifacts should be placed under `evidence/mac-parity-qa-20260620/integrations/`, `browser-extension/`, or referenced here:

| Scenario cluster | Expected proof | Status |
| --- | --- | --- |
| Chrome extension/native host | disposable profile record or explicit gate, manifest/native host evidence, live connected/disconnected proof | Pending Lane C |
| Computer Use/Appshots | docs/platform classification, UI inspect evidence, no helper-install proof | Pending Lane C |
| MCP/plugins/Sites/IDE/web search/image/memory/automation | entitlement/config/gate classification; synthetic cleanup where run | Pending Lane C |
| Notifications/prevent sleep/remote control | setting/session evidence or gate classification, cleanup | Pending Lane C |
| Security rows | loopback binding, app-server transport, `--no-sandbox`, approval/sandbox/redaction notes | Pending Lane C |
| Final architect/critic review | severity-rated findings and approval/block verdict | Pending Lane C |

## Merge rule

Lane evidence should update row statuses without weakening the taxonomy. A Linux PASS seed stays `PARTIAL` until macOS/docs oracle coverage and cleanup proof are complete. A denied gate stays `GATED`, not `FAIL`.
