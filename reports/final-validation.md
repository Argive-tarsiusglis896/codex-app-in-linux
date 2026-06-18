# Codex Desktop Linux workaround validation

## Result

PASS for local non-system proof: the `the wrapper project` wrapper built a Linux Electron Codex app from a frozen upstream Codex DMG, and the generated app started on Linux with Electron renderer processes and a loopback webview server.

Screenshot capture was attempted but blocked by the GNOME/Wayland session (`org.freedesktop.DBus.Error.AccessDenied`) and X root capture failed with `BadMatch`. Functional visual/manual interaction beyond launch was therefore not completed in this headless/tool-controlled pass.

## Source and artifact pinning

- Wrapper repo: `<wrapper-source-url>`
- Wrapper commit used for successful build: `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`
- Upstream DMG URL: `https://persistent.oaistatic.com/codex-app-prod/Codex.dmg`
- DMG size: `486144300`
- DMG SHA256: `31d8e2666a0895a830df0832dc4083ae82a6e9bd26603141c0293acea6618211`
- Upstream app version from build-info: `26.611.62324`
- Electron version from build-info: `42.1.0`
- Target: Ubuntu 24.04.4 LTS, x64, GNOME Wayland, `.deb` family profile

## Commands actually used for successful proof

Allowed non-system proof path:

```bash
git clone <wrapper-source-url> upstream/codex-linux-wrapper
# frozen DMG was downloaded by ranged HTTPS and checksum recorded
make build-app DMG=/home/yun/tmp/codex-linux-app-proof/evidence/upstream/Codex.dmg
./codex-app/start.sh
```

No `sudo`, `pkexec`, package manager, service enablement, native package install, or updater command was needed for the successful proof.

## Build evidence

- Generated app directory exists: `upstream/codex-linux-wrapper/codex-app/`
- Build metadata copied to `evidence/build/build-info.json`
- Patch report copied to `evidence/build/patch-report.json`
- Build output showed:
  - system dependencies found using `7z`
  - provided frozen DMG used
  - ASAR extracted and patched
  - native modules rebuilt for Electron `42.1.0`
  - Linux Computer Use backend compiled
  - `codex-app/start.sh` generated

## Launch / smoke evidence

Evidence files:

- `evidence/gui/process-evidence.txt`
- `evidence/gui/webview-smoke-result.txt`
- `evidence/gui/gnome-screenshot-result.txt`
- `evidence/gui/codex-app-window-probe.log`
- `evidence/gui/codex-app-webview-smoke.log`

Observed during launch:

- `start.sh` process stayed alive until smoke-test termination.
- Webview server process started on loopback only:
  - `python3 ... webview-server.py 5175 --bind 127.0.0.1`
- Electron main process started:
  - `electron --no-sandbox --class=codex-desktop --app-id=codex-desktop ...`
- Electron zygote/gpu/utility/renderer processes started.
- Local webview responded on `http://127.0.0.1:5175/` with HTTP 200 and HTML prefix:
  - `<!doctype html><html lang="en" ...>`

This proves the generated Linux app launches its local UI server and Electron runtime on this Linux host.

## Manual QA status

Completed:

- Audit gate and allowlist/denylist.
- Frozen upstream DMG checksum.
- Local non-system build.
- Generated app launch smoke.
- Loopback-only webview server check.
- Electron process tree check.

Not completed:

- Visual screenshot proof, because GNOME/Wayland denied screenshot over DBus and X root capture failed.
- Login/sign-in QA.
- Disposable project/thread/shell approval QA.

## Security notes

Denied for this proof and still not run:

- `make bootstrap-native`
- `make install-native`
- `make setup-native`
- `make update-native`
- `make package`
- `make install`
- `make deb/rpm/pacman/appimage`
- `sudo`, `pkexec`, package managers, `systemctl`, service/updater enablement

Risk surfaces confirmed by audit:

- The wrapper downloads/transforms proprietary upstream DMG content.
- The generated app bundles/stages plugin resources and Linux Computer Use backend resources.
- Native packages introduce updater/service/polkit surfaces; those were intentionally avoided.
- Webview server bind was loopback (`127.0.0.1`) in the successful launch evidence.

## Recommendation

Use this wrapper path when the immediate goal is opening Codex Desktop GUI locally on Linux. Keep using the local generated app first. Do not install the native package or enable updater/service surfaces until visual/login/manual QA is finished.

For daily supported work, keep the official fallback ready: Codex CLI on Linux, or official Codex App on macOS/Windows connected to this Linux host over SSH.
