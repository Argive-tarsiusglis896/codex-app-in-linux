# Feature matrix
Updated after final web-search result screenshot. Canonical CSV: `feature-matrix.csv`.
| Feature / scenario | Linux wrapper status and evidence | Parity verdict | Evidence owner |
| --- | --- | --- | --- |
| Startup/window/render/version pins | Seed PASS: renderer mounted, ready message handled, app-server handshake, version 26.616.41845/Electron 42.1.0 in reports/final-validation.md and latest-upgrade-summary.json | PASS | Lane B |
| Account readiness / existing signed-in session | Seed PASS: account/read succeeded for existing signed-in chatgpt user in reports/final-validation.md and interactive-qa-summary.json | PASS | Lane B |
| Fresh sign-in / unauthenticated login | GATED_NOT_RUN: clean login would require logout/credential flow; existing account session tested separately | GATED | Lane B |
| Project selection / file dialog | Clicking Project/Work in a project via CDP produced no visible picker; earlier menu-only evidence remains partial | FAIL | Lane B |
| Basic local chat in project | Seed PASS: controlled prompt returned codex-linux-qa-ok; no inputSchema error | PASS | Lane B |
| Multitask / multi-chat / chat search | PASS for chat search; multi-chat depth remains partial/not fully expanded | PARTIAL | Lane B |
| Local mode harmless terminal/action approval | Seed PASS: echo codex-linux-shell-ok ran; deny/cancel behavior pending Lane B | PASS | Lane B |
| Sandbox/project-root boundaries | Seed UNTESTED for explicit boundary probes | UNTESTED | Lane C |
| Settings harmless toggle and restart persistence | Settings panel did not open via CDP; restart persistence not testable in app UI | FAIL | Lane B |
| Sidebar, sources, task summaries, generated artifact preview | UNTESTED_BLOCKED: no safe generated artifact workflow completed; add-files menu present but picker/upload not exercised | UNTESTED | Lane B |
| Built-in Git diff/review/stage/unstage/revert | ENV_BLOCKED: project picker failed/not exercisable, so disposable repo review UI could not be reached safely | ENTITLEMENT/ENV BLOCKED | Lane B |
| Local commit from app | ENV_BLOCKED: project picker failed/not exercisable, so app local commit UI could not be reached safely | ENTITLEMENT/ENV BLOCKED | Lane B |
| Push / pull request creation | GATED: production remote push/PR denied by team brief | GATED | Lane B/C |
| Worktree mode | ENV_BLOCKED: project picker failed/not exercisable, so worktree mode could not be reached safely | ENTITLEMENT/ENV BLOCKED | Lane B |
| Automations / thread automations | Automations page opens and shows Create your first automation, Daily brief, Weekly review, Project monitor; no automation created | PARTIAL | Lane C |
| Skills discovery/invocation | Add files menu exposes Agents list and Plugins page exposes Skills tab/catalog; no skill invoked | PARTIAL | Lane C |
| MCP settings / stdio local mock | GATED_NOT_RUN: Settings/MCP UI not reachable via CDP; no manual config mutation performed | GATED | Lane C |
| Plugins | Plugins page opens and catalog loads with search, Added/Manage, OpenAI/workspace/personal sections | PASS | Lane C |
| Sites plugin | UNTESTED in Linux wrapper seed | GATED | Lane C |
| IDE Extension sync | UNTESTED in Linux wrapper seed | UNTESTED | Lane C |
| Web search | PASS: Web search returned WEB_SEARCH_STATUS available and cited Codex app - OpenAI Developers; screenshot final-ui-state.png captured result. | PASS | Lane C |
| Image input | Add files menu exposes Files and folders; no native file picker/input exercised | PARTIAL | Lane C |
| Image generation/editing | Not run due entitlement/cost gate | GATED | Lane C |
| Memories | GATED_NOT_RUN: synthetic memory would mutate persistent account memory and needs cleanup proof | GATED | Lane C |
| Notifications | GATED_NOT_RUN: desktop notification permission/background behavior not mutated | GATED | Lane C |
| Prevent sleep while running | GATED_NOT_RUN: sleep inhibitor/system behavior not toggled | GATED | Lane C |
| In-app browser preview/comments | Browser Use pass on public page; comments/annotation not exercised | PARTIAL | Lane B/C |
| Browser Use direct operation | Seed PASS: Browser Use returned Example Domain from https://example.com | PASS | Lane B/C |
| Developer Mode CDP inspection | UNTESTED in Linux wrapper seed | GATED | Lane C |
| Chrome extension native-host live connection | PARTIAL_GATED: Chrome installed/running; disposable HOME lacks native-host manifest; no disposable extension Connected proof | PARTIAL | Lane C |
| Computer Use | Disabled/not exposed in app QA; official docs macOS/Windows only | NOT SUPPORTED | Lane C |
| Appshots | ENV_BLOCKED_PLATFORM: Linux UI did not expose successful Appshots; prior GNOME/Wayland screenshot capture blocked | ENTITLEMENT/ENV BLOCKED | Lane C |
| Remote connections / mobile steering | Codex mobile page opens but copy says Connect your phone to this Mac on Linux; no device connected | PARTIAL | Lane C |
| Voice dictation | Dictate button present; microphone permission/use not exercised | PARTIAL | Lane C |
| Floating pop-out window / stay on top | UNTESTED_NO_VISIBLE_CONTROL: no pop-out control visible in CDP-observed UI | UNTESTED | Lane B |
| Webview loopback binding and app-server transport | Seed PASS: webview server bound to 127.0.0.1:5175; app-server handshake succeeded | PASS | Lane C |
| Electron sandbox posture | Seed WRAPPER CAVEAT: Electron launches with --no-sandbox in process evidence | WRAPPER DEFECT | Lane C |
| Native updater/service/package surfaces | Seed GATED/DENIED: make bootstrap-native/install-native/setup-native/update-native/package/install/deb/rpm/pacman/appimage and sudo/pkexec/package-manager/systemctl not run | GATED | Lane A/C |
