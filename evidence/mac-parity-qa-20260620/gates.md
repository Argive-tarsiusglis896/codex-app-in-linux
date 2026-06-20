# Gates ledger

## Global denylist — active for entire run

| Gate | Status | Owner | Reason | Maximum blast radius if later approved | Rollback / cleanup |
| --- | --- | --- | --- | --- | --- |
| `sudo` | DENIED | Team | Prevent host/system mutation | Host-wide privilege escalation | N/A; not run |
| `pkexec` | DENIED | Team | Prevent privileged desktop mutation | Host-wide privilege escalation | N/A; not run |
| Package manager (`apt`, `dpkg`, etc.) | DENIED | Team | No native install/package QA in this plan | Host package database mutation | N/A; not run |
| `systemctl` / service enablement | DENIED | Team | No updater/service installation | User/system service mutation | N/A; not run |
| Native package install/updater/service | DENIED | Team | Separate privileged/system QA plan required | App updater/service/autostart state | N/A; not run |
| Personal browser profile | DENIED | Team | Prevent cookie/session/profile leakage | Browser profile and signed-in sites | Use disposable profile only; delete profile |
| Production remote push/PR | DENIED | Team | Prevent repository publication side effects | Remote repo branch/PR mutation | Disposable remote only if separately approved |
| Sensitive signed-in websites | DENIED | Team | Prevent account/data exposure | External account state | Use public/local non-auth pages only |
| Computer Use helper install | DENIED | Team | Helper install is a native/system mutation | Host GUI automation privileges | N/A; inspect-only |
| Wrapper/source-code edits | DENIED | Team | QA-only run | Product source changes | N/A; leave artifacts/reports only |
| Commit or push | DENIED | Team | QA artifacts only, no repository publication | Git history/remote state | N/A; no commit/push |

## Conditional gates requiring explicit approval record

| Gate | Default | Safe substitute |
| --- | --- | --- |
| Developer Mode CDP | GATED | Docs-only or read-only UI inspection without enabling CDP |
| Chrome extension live test | GATED to disposable profile only | Manifest/native-host inspect and disconnected status |
| MCP HTTP/OAuth | GATED | stdio local mock without secrets |
| Plugin install/update | GATED | Read-only entitlement/UI inspection |
| Sites deployment | GATED | Docs-only entitlement record |
| Image generation | GATED due entitlement/cost | Image input with synthetic local image |
| Automations | GATED unless cleanup proof can be captured | UI/read-only discovery or no-op schedule not created |
| Remote control / mobile steering | GATED | Docs-only/account setting inspection |
| Notifications/prevent sleep | GATED to harmless setting probes | D-Bus/session read-only evidence |

## Current run decisions

- No denied global gate has approval in Lane A artifacts.
- Existing Linux seed evidence already states native/system commands were not run.
- Lanes B and C must record any gate request outcome here before executing a gated scenario.
