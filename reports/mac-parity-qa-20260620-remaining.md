# Remaining Codex Linux-wrapper parity QA — Ultragoal G002

Status: COMPLETE FOR SAFE REMAINING SCOPE. No denied native/system/browser-profile/production gates were crossed.

## Evidence

- UI/CDP surface results: `evidence/mac-parity-qa-20260620/remaining/ui-surface-results.json`
- CDP launch logs: `evidence/mac-parity-qa-20260620/remaining/launch-cdp.log`
- Current Sentry scope copy: `evidence/mac-parity-qa-20260620/remaining/scope_v3.json`
- Current launcher log tail: `evidence/mac-parity-qa-20260620/remaining/launcher-port-5175-current.log`

## Added coverage

| Surface | Result | Evidence |
| --- | --- | --- |
| Plugins catalog | PASS | Plugins page opened with catalog/search and OpenAI/workspace/personal sections. |
| Automations UI | PARTIAL | Automations page opened with Create your first automation/templates; no recurring task created. |
| Codex mobile / remote control | PARTIAL / WRAPPER TEXT BUG | Page opened, but says "Connect your phone to this Mac" on Linux; no device connected. |
| New chat composer | PASS | Composer returned with Custom mode, model selector, Dictate, Add files, Work in a project. |
| Add files / agents menu | PASS MENU ONLY | Menu showed Files and folders, Goal, Plan mode, Agents list, Files/chats search. Native file picker not exercised. |
| Web search | PASS | App returned `WEB_SEARCH_STATUS: available` and cited `Codex app - OpenAI Developers`; screenshot captured in `evidence/mac-parity-qa-20260620/remaining/final-ui-state.png`. |
| Project picker | FAIL / NOT EXERCISABLE VIA CDP | Project/Work in a project click produced no visible picker in CDP session. |
| Settings | FAIL / NOT EXERCISABLE VIA CDP | Settings button present but did not open visible panel via CDP. |
| Voice dictation | PRESENT / GATED | Dictate button present; microphone permission not used. |
| Image/file input | PARTIAL / GATED | Add files menu present; native picker/upload not exercised. |

## Final classification update

The remaining QA improves UI coverage but does not change the overall verdict: core local workflows are usable, but full macOS/Windows app parity is not established. New hard findings are: Project picker and Settings are not successfully exercisable through the current Linux/CDP path, web search can invoke search but did not produce a final response, and mobile remote-control copy incorrectly names Mac on Linux.

## Gates preserved

No sudo, pkexec, package-manager, systemctl, native install/updater/service/package, personal Chrome profile, production remote push/PR, sensitive signed-in website, Computer Use helper install, source edit, commit, or push was performed.


## Explicit dispositions for initially omitted G002 surfaces

The architect review required explicit closure language for surfaces not directly tested. These are now recorded in `evidence/mac-parity-qa-20260620/remaining/blocker-dispositions.json` and the feature matrix.

- Fresh login/session persistence: GATED_NOT_RUN; preserving current account state, no logout/credential mutation.
- Git/worktree/review/local commit: ENV_BLOCKED because Project/Work in a project did not open a usable picker in CDP, so disposable repo UI could not be reached safely.
- Chrome disposable-profile status: PARTIAL_GATED; Chrome installed/running and clean disposable HOME lacks native-host manifest; no extension Connected proof without gated setup.
- MCP local mock: GATED_NOT_RUN because Settings/MCP UI was not reachable and manual config mutation would not prove app UI.
- Memories: GATED_NOT_RUN because persistent memory mutation needs explicit cleanup proof.
- Notifications: GATED_NOT_RUN because desktop notification permissions/background behavior were not mutated.
- Prevent sleep: GATED_NOT_RUN because sleep inhibitor/system behavior was not toggled.
- Appshots: ENTITLEMENT/ENV BLOCKED; Linux UI did not expose a successful Appshots path and prior GNOME/Wayland capture was blocked.
- Artifact preview: UNTESTED_BLOCKED; no safe generated-artifact workflow completed after project picker/file input limits.
- Floating pop-out/stay-on-top: UNTESTED_NO_VISIBLE_CONTROL in CDP-observed UI.
