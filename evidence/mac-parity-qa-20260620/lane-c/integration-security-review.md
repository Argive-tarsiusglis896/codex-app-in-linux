# Lane C — Integrations, security, and final review

Run: `mac-parity-qa-20260620`  
Worker: `worker-3`  
Scope: QA-only classification for Linux-wrapper integrations and security rows. No wrapper/source code was edited.

## Evidence produced

- Command capture bundle: `evidence/mac-parity-qa-20260620/lane-c/commands/command-results.json`
- Browser/native-host command stdout/stderr: `evidence/mac-parity-qa-20260620/lane-c/commands/`
- Redacted/static manifest snapshots: `evidence/mac-parity-qa-20260620/lane-c/manifests/`
- Snapshot index with SHA-256 hashes: `evidence/mac-parity-qa-20260620/lane-c/manifest-index.json`

## Commands and inspections

| Check | Result | Evidence |
| --- | --- | --- |
| Installed Chrome-family browsers | Google Chrome installed at `/usr/bin/google-chrome`; xdg default browser is `google-chrome.desktop`. | `commands/installed-browsers.stdout` |
| Chrome running detector | A `chrome` process set is running, but this was not opened or controlled by this lane and is not treated as disposable-profile evidence. | `commands/chrome-is-running.stdout` |
| Native-host manifest with disposable `HOME` | Manifest absent under the disposable home; script exited `1` as expected for a clean profile. | `commands/native-host-disposable-home.stdout` |
| Plugin manifests | Browser and Chrome bundled plugins present; Computer Use plugin resources present as Linux alpha wrapper resource. | `manifests/browser-plugin.json`, `manifests/chrome-plugin.json`, `manifests/computer-use-plugin.json` |
| App build/security metadata | Upstream app `26.616.41845`, Electron `42.1.0`; `linuxFeatures.enabled` is empty. | `manifests/build-info.json`, `manifests/linux-features-staged.json` |

## Classification matrix — integrations

| Feature / scenario | Official source | Official support / eligibility | Linux wrapper evidence | Verdict | Risk/gate / cleanup |
| --- | --- | --- | --- | --- | --- |
| In-app Browser / Browser Use on public non-auth pages | https://developers.openai.com/codex/app/browser | For local development servers, file-backed previews, public pages without sign-in; Browser plugin required. | Existing run passed Browser Use on `https://example.com` returning `Example Domain`; Browser plugin manifest is bundled. | PASS for safe non-auth Browser Use | No signed-in sites; no secrets entered. |
| Chrome extension native host, disposable profile | https://developers.openai.com/codex/app/chrome-extension | Chrome extension + native host; intended for signed-in browser state with website prompts. | Chrome is installed, but disposable `HOME` lacks native-host manifest and no extension Connected screen was produced. Running Chrome processes were not controlled because profile provenance was not disposable. | PARTIAL / GATED | Personal browser profile denied. Live Connected proof remains gated to disposable profile with extension installed. |
| Chrome extension using personal profile | https://developers.openai.com/codex/app/chrome-extension | Requires same Chrome profile where extension is installed; can access signed-in data. | Not exercised. Existing evidence mentions configured Chrome Profile 1, but this lane did not inspect/control a personal profile. | GATED | Personal browser profiles and sensitive signed-in websites denied. |
| Computer Use | https://developers.openai.com/codex/app/computer-use | Official docs state supported regions on macOS and Windows; install plugin; macOS needs Screen Recording/Accessibility permissions. | Linux wrapper bundles a third-party/Linux-alpha `computer-use` plugin and MCP command, but app QA summary says tool surface disabled/not exposed; no helper install or sudo attempted. | NOT SUPPORTED for official parity; wrapper resource is inspect-only | Computer Use helper install/system permissions denied. Do not label this as a Linux app failure. |
| Appshots | Official features page mentions Appshots only indirectly in app/log surface; no Linux support claim in approved oracle. | Platform-specific/likely non-parity lane. | Log line: `Appshot hotkey inactive configured=true enabled=true platform=linux`; no UI use or screenshot proof. | UNTESTED / GATED | Screen capture on GNOME/Wayland already blocked in prior evidence; do not overclaim. |
| MCP settings | https://developers.openai.com/codex/app/features#mcp-support | App/CLI/IDE share MCP settings; configure in app settings. | Computer Use plugin `.mcp.json` exists; no safe mock MCP server was run in this lane. | UNTESTED | OAuth/remote credentials gated; local mock could be a future safe lane. |
| Plugins / skills discovery | https://developers.openai.com/codex/app/features | Skills support and plugins surfaced in app; recommended skills may require network/git. | Bundled Browser/Chrome/Computer Use plugin cache sync logged. Recommended skills refresh failed in prior log due local git wrapper path, not feature proof. | PARTIAL | Plugin install/update is gated; no package install performed. |
| Sites plugin | Official plan seed; no safe entitlement evidence found in this run. | Entitlement/admin and deployment dependent. | No Linux evidence in this lane. | UNTESTED / ENTITLEMENT-ENV BLOCKED | Disposable deployment and credentials required. |
| IDE sync | https://developers.openai.com/codex/app/features#sync-with-the-ide-extension | Requires IDE Extension installed in same project. | No supported IDE fixture used. | UNTESTED / GATED | External IDE/profile fixture required. |
| Web search | https://developers.openai.com/codex/app/features#web-search | First-party web search available; behavior depends on sandbox/full-access config. | No app-thread web-search prompt executed in this lane. | UNTESTED | Network/data-control policy row only. |
| Image input | https://developers.openai.com/codex/app/features#image-input | Composer can accept dropped images / screenshot tools. | Synthetic image flow not run. | UNTESTED | Safe synthetic fixture needed. |
| Image generation | https://developers.openai.com/codex/app/features#image-generation | Uses `gpt-image-2`, counts against usage. | Not run to avoid entitlement/cost ambiguity. | GATED | Cost/entitlement gate required. |
| Memories | https://developers.openai.com/codex/app/features#memories | Availability dependent. | No synthetic add/use/remove cleanup proof. | UNTESTED | Requires explicit cleanup proof. |
| Automations | https://developers.openai.com/codex/app/features#automations | Recurring/project/thread automations supported where available. | No schedule created; no cleanup needed. | UNTESTED | Avoided unattended recurring work. |
| Notifications | https://developers.openai.com/codex/app/features#notifications | Desktop notifications configurable in app settings. | No DBus/portal notification emission tested. | UNTESTED | Desktop-session permission lane. |
| Prevent sleep | https://developers.openai.com/codex/app/features#keep-your-computer-awake | Settings toggle to prevent sleep while running. | Not toggled; no logind/portal inhibitor proof. | UNTESTED | Session/system-power side effects gated. |
| Remote control | https://developers.openai.com/codex/remote-connections | Account/device gated. | Log shows remote control authorization check failed because app was not signed in in that prior disposable run; no devices added. | ENTITLEMENT/ENV BLOCKED | No account/device authorization mutation. |

## Classification matrix — security rows

| Security row | Linux wrapper evidence | Verdict | Notes |
| --- | --- | --- | --- |
| Webview loopback binding | Existing process evidence shows `webview-server.py 5175 --bind 127.0.0.1`; launch log serves only `127.0.0.1` requests. | PASS | Keep this as a required regression check. |
| App-server transport | Launch log shows `[AppServerConnection] Starting app-server connection hostId=local transport=stdio` and handshake success. | PASS | Local stdio transport, not a network listener. |
| Electron sandbox caveat | Process evidence shows top-level Electron launched with `--no-sandbox`; child processes also include `--no-sandbox` despite some renderer `--enable-sandbox` flags. | WRAPPER DEFECT / SECURITY CAVEAT | This is not macOS parity and must remain visible in final report. |
| Approval/sandbox boundaries | Existing QA passed harmless shell command and plan records approval/sandbox documentation; deny/cancel behavior was not separately re-exercised by this lane. | PARTIAL | Final report should avoid claiming full approval parity. |
| Native install/updater/service/package denylist | No `sudo`, `pkexec`, package-manager, `systemctl`, native package, updater, or service command was run by this lane. Existing final validation confirms those remained denied. | PASS | Continue to keep privileged install lanes out of scope. |
| Redaction / sensitive data | This lane stored command JSON, plugin manifests, build metadata, and references to existing reports. No personal Chrome profile contents, cookies, tokens, signed-in websites, or secrets were captured. | PASS | Existing process evidence contains home paths and should be treated as local-machine metadata before publication. |

## Final review status

Architect/critic status for lane C: **WATCH**, not CLEAR. The safe evidence is adequate to classify Browser Use, native-host disposable-profile absence, app-server transport, loopback webview binding, and `--no-sandbox`. It is not adequate for a full parity declaration because Chrome Connected proof, Computer Use, Appshots, MCP, Sites, IDE sync, web search, image, memories, automation, notifications, prevent-sleep, and remote-control remain untested, gated, or environment-blocked.

Cross-lane inspection performed by this lane found Lane A's report skeleton still marked Lane B/C pending at inspection time, while Lane B's report classified safe local generated-app QA as PASS WITH LIMITATIONS. That is consistent with this lane's WATCH review: core local flows can be reported as working with limitations, but integration-heavy and security-sensitive parity must remain non-clear until stronger direct evidence exists.

Recommended final-report language: "The Linux wrapper demonstrates local generated-app viability for core safe flows and in-app Browser Use, but does not establish official macOS/Windows feature parity. Integration-heavy surfaces are mostly untested/gated, and Electron `--no-sandbox` is a material wrapper security caveat."
