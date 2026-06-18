# Worker 4 Wrapper Audit

## Source pin
- Repository: <wrapper-source-url>
- Commit: `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`
- Audit date: 2026-06-18

## Upstream artifact metadata
- Default DMG URL from `scripts/lib/dmg.sh`: `https://persistent.oaistatic.com/codex-app-prod/Codex.dmg`
- HEAD status: `200`
- ETag: `0x8DECC801870CD77`
- Last-Modified: `Wed, 17 Jun 2026 14:52:47 GMT`
- Content-Length: `486144300`
- Content-Type: `application/x-apple-diskimage`

## First-proof allowlist
Only these actions are approved for this worker's non-system proof:
1. Read-only inspection of wrapper files and generated reports.
2. Pinned checkout at commit `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`.
3. Dependency preflight using already-present tools only.
4. Network fetches performed by the audited local build path for:
   - upstream Codex DMG from the default HTTPS URL,
   - managed Node runtime from `https://nodejs.org/dist/v22.22.2/node-v22.22.2-linux-x64.tar.xz` with pinned SHA256 in `scripts/lib/node-runtime.sh`,
   - Electron/runtime and rebuild assets used by `install.sh`/`@electron/rebuild`.
5. `make build-app-fresh` or equivalent `./install.sh --fresh` in the cloned wrapper checkout.
6. Local launch using `make run-app` or `./codex-app/start.sh` after successful build.
7. Log, screenshot, checksum, build-info, patch-report, and manual QA evidence capture.

## Denylist / stop conditions
Do not run without separate approval:
- `sudo`, `pkexec`, package managers, `systemctl`, service enable/start commands.
- `make bootstrap-native`, `make install-native`, `make update-native`, `make setup-native`, `make package`, `make install`, `make service-enable`, `make deb`, `make rpm`, `make pacman`, `make appimage`.
- `git pull --ff-only` through `make update-native` or other moving-checkout update paths.
- `bash scripts/install-deps.sh`, `rustup`, NodeSource/system Node bootstrap, or any system dependency installation.
- Optional Linux feature enablement and package hooks.

## Risk findings
- `Makefile` local proof targets `build-app-fresh` and `run-app` do not invoke sudo/package-manager/service paths, but native/package targets do.
- `install.sh` downloads and transforms the upstream DMG, downloads/copies a managed Node runtime, patches ASAR, downloads Linux Electron, stages bundled plugin resources, runs Linux feature hooks, and writes `codex-app/start.sh`.
- Default optional Linux features are disabled when `linux-features/features.json` is absent. Linux Computer Use backend resources may be bundled/registered by default but UI controls remain disabled until opt-in per docs.
- Launcher serves webview assets on `127.0.0.1` by default (`5175`) and uses a generated local start script.
- Native packages include updater/service/polkit surfaces; package generation and installation are denied for this proof.

## Dependency preflight result
Present: `python3`, `curl`, `unzip`, `tar`, `make`, `g++`, `git`, `sha256sum`, `7z`, `codex`.
Missing: `7zz` only; `7z` is present and accepted by `check_deps`.
Codex CLI baseline: `codex-cli 0.136.0` at `/home/yun/.npm-global/bin/codex`.
Session: Wayland (`WAYLAND_DISPLAY=wayland-0`, `DISPLAY=:0`).

## Decision
Audit gate passed for non-system local proof using only the allowlisted generated-app build/run path. Package install, updater, service, dependency installation, and optional feature enablement remain denied.
