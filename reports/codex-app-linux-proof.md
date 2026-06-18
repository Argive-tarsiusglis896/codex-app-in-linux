# Codex Desktop Linux wrapper proof report

Date: 2026-06-18
Worker: worker-1

## Verdict

Local generated-app proof passed to sign-in-ready/GUI-ready state without system install. The generated Electron app started on Ubuntu Wayland, served webview assets on `127.0.0.1:5175`, spawned the local Codex CLI over stdio, completed the CLI initialize handshake, and logged `window ready-to-show`. Interactive authenticated QA was not completed because the proof used a disposable unauthenticated HOME by design.

## Pinned inputs

- Wrapper repo: <wrapper-source-url>
- Wrapper commit: 9125911c8347c35177dfc76e2f5bce2b8b2e41d4
- Wrapper commit URL: <wrapper-source-url>/commit/9125911c8347c35177dfc76e2f5bce2b8b2e41d4
- Wrapper version: 0.8.2
- Upstream DMG URL: https://persistent.oaistatic.com/codex-app-prod/Codex.dmg
- Upstream DMG SHA256: 31d8e2666a0895a830df0832dc4083ae82a6e9bd26603141c0293acea6618211
- Upstream DMG size: 486144300 bytes
- Upstream app version: 26.611.62324
- Electron version from DMG/build: 42.1.0
- Local frozen DMG path during proof: /home/yun/tmp/codex-proof-work/artifacts/Codex.dmg

## Host/runtime

- OS target: ubuntu:24.04/deb:ubuntu+gnome
- Desktop/session: ubuntu:GNOME / wayland
- Arch: x64
- Codex CLI: `codex-cli 0.136.0` at `/home/yun/.npm-global/bin/codex`
- Dependency preflight: python3, curl, unzip, make, g++, cargo, rustc, codex, node, npm, git, sha256sum present; `7zz` missing but `7z 23.01` present and accepted by installer.

## Commands run

Allowed local proof commands only:

1. `git clone <wrapper-source-url> /home/yun/tmp/codex-proof-work/codex-linux-wrapper`
2. `curl -fsSLI https://persistent.oaistatic.com/codex-app-prod/Codex.dmg -o evidence/upstream-dmg.headers`
3. `curl -fL --retry 3 --continue-at - -o /home/yun/tmp/codex-proof-work/artifacts/Codex.dmg https://persistent.oaistatic.com/codex-app-prod/Codex.dmg`
4. `make inspect-upstream DMG=/home/yun/tmp/codex-proof-work/artifacts/Codex.dmg REBUILD_REPORT_DIR=/home/yun/tmp/codex-proof-work/reports/inspect`
5. `make build-app-fresh DMG=/home/yun/tmp/codex-proof-work/artifacts/Codex.dmg ...`
6. `timeout 75s /home/yun/tmp/codex-proof-work/codex-linux-wrapper/codex-app/start.sh --new-instance -- /home/yun/tmp/codex-proof-work/disposable-home/disposable-project` with disposable HOME/XDG dirs.

No sudo, pkexec, package-manager install, native package install, bootstrap-native, install-native, update-native, make install, make package, service-enable, systemctl/service mutation, or updater enablement was run.

## Observed launch evidence

- `evidence/logs/launcher-port-5175.log`: webview server started on localhost, assets returned HTTP 200, Electron spawned, app server connection initialized, and main window reached ready-to-show.
- Run command status: `exit_status=124`. Status 124 is expected from the external timeout after the app stayed running for the smoke window.
- Launcher log highlights:
  - `Webview origin verified.`
  - `Launching app ... enableUpdater=false ... platform=linux`
  - `Starting app-server connection hostId=local transport=stdio`
  - `Codex CLI initialized`
  - `window ready-to-show appearance=primary`
  - unauthenticated disposable state logged: `Sign in to ChatGPT in Codex Desktop to check remote control authorization.`
- Non-fatal observations: VAAPI warning; recommended-skills refresh failed because disposable HOME changed the custom git wrapper's expected path; this did not block CLI initialization or window readiness.

## Screenshot evidence

Screenshot capture was attempted but not available in this session:

- `gdbus org.gnome.Shell.Screenshot.Screenshot` returned `AccessDenied: Screenshot is not allowed`.
- `xwd -root` on the Wayland/XWayland desktop returned `BadMatch` and produced no usable PNG; `ffmpeg` also lacks xwd input support here.
- Attempt logs are in `evidence/logs/screenshot-*`. Use manual QA checklist below for visual confirmation in an interactive desktop session.

## Feature/surface audit summary

- Local webview binds to `127.0.0.1` by default (`launcher/webview-server.py` and launcher `WEBVIEW_ORIGIN`).
- App-server transport in the successful proof was `stdio`, not public TCP.
- Generated local app logs `enableUpdater=false` and no package updater service was installed.
- Linux Computer Use UI patch status: skipped-disabled; plugin/backend resources were still staged by default.
- Chrome native messaging manifests are written under user browser config locations at app startup when host resources exist; inventory before sensitive-browser testing.
- Build info reports `linuxFeatures.enabled: []`.
- Patch report status counts: {'applied': 56, 'already-applied': 6, 'skipped-disabled': 3}.

## Artifacts

- `evidence/upstream-dmg.headers`
- `evidence/upstream-dmg.sha256`
- `evidence/reports/build-info.json`
- `evidence/reports/patch-report.json`
- `evidence/reports/rebuild-report.json`
- `evidence/reports/inspect-patch-report.json`
- `evidence/reports/inspect-rebuild-report.json`
- `evidence/logs/build-command.log`
- `evidence/logs/inspect-command.log`
- `evidence/logs/launcher-port-5175.log`
- `evidence/logs/run-command.status`

## Recommendation

Do not run native package install yet. The local generated app proof is good enough to proceed to a manual authenticated GUI QA pass in a disposable account/session. Keep updater/package/service paths denied until separate approval. If authenticated QA or wrapper trust is unacceptable, use the official fallback: Codex App on macOS/Windows connected to Linux via SSH plus the official Linux CLI.
