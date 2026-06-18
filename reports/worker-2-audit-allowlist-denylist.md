# Worker 2 wrapper audit: allowlist / denylist

Task: `task-2` in `execute-approved-plan-to-valid-ac009d6e`.

Wrapper source audited: `<wrapper-source-url>`, cloned at commit `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`.

## Audit scope

Inspected the approved ralplan artifacts plus upstream `README.md`, `Makefile`, `install.sh`, `scripts/lib/install-helpers.sh`, `scripts/lib/dmg.sh`, `scripts/lib/node-runtime.sh`, `scripts/lib/native-modules.sh`, `launcher/start.sh.template`, `launcher/webview-server.py`, `scripts/install-deps.sh`, and dangerous-command search results for sudo/package-manager/systemd/git-pull paths.

## First-proof allowlist

Allowed for this worker's proof only:

- Read-only inspection of wrapper files and plan artifacts.
- `git clone <wrapper-source-url> upstream/codex-linux-wrapper` and `git -C upstream/codex-linux-wrapper rev-parse HEAD` to pin source.
- Dependency preflight using already installed tools only: `python3`, `curl`, `unzip`, `tar`, `make`, `g++`, `git`, `7z/7zz`, and optional `codex --version`.
- Freeze upstream DMG locally with `curl -L --fail` from `https://persistent.oaistatic.com/codex-app-prod/Codex.dmg`, then record `sha256sum`, headers/metadata, file size, and path.
- Local generated-app build only: `make build-app DMG=<absolute frozen Codex.dmg path>` from the pinned checkout. This uses `install.sh` without package targets.
- Local inspection target: `make inspect-upstream DMG=<absolute frozen Codex.dmg path>` if needed for reports.
- Local launch only after generated app exists: `make run-app` or `./codex-app/start.sh` with logs under XDG cache/state. No install target.
- Screenshot/log capture and disposable-project QA if the GUI opens.

## Denylist / stop conditions

Denied without separate approval:

- `sudo`, `pkexec`, `systemctl`, package managers, service enablement, autostart/timer enablement, or any system-wide mutation.
- `make bootstrap-native`, `make install-native`, `make setup-native`, `make update-native`, `make install`, `make package`, `make deb`, `make rpm`, `make pacman`, `make appimage`, `make service-enable`, `make service-status`, updater paths, and contrib user-local installer paths.
- `bash scripts/install-deps.sh`, `rustup`, NodeSource setup, apt/dnf/pacman/zypper installs, or distro dependency bootstrap.
- `git pull --ff-only` / update-native paths after pinning the checkout.
- Enabling optional Linux features or Computer Use UI. The Linux Computer Use backend may be bundled by the wrapper, but the in-app UI stays disabled unless explicitly approved.
- Public binding or shared transport. The reviewed webview server defaults to `127.0.0.1`; any non-loopback bind is a blocker.

## Notable audited behavior

- `Makefile` confirms `build-app` and `build-app-fresh` call `./install.sh` and produce `codex-app/`; `run-app` calls `codex-app/start.sh`.
- `Makefile` confirms denied package/install/update/service targets: `bootstrap-native` runs dependency install then install-native; `install-native` builds, packages, installs; `update-native` runs `git pull --ff-only`; `install` invokes sudo package managers; `service-enable` invokes systemctl user service enablement.
- `install.sh` downloads/extracts the DMG, downloads a managed Node runtime, downloads Electron, rebuilds native modules via npm, patches ASAR, writes build-info/patch-report, and creates a local `start.sh`. This is acceptable for local proof when using a frozen DMG and no package/install target.
- `scripts/lib/dmg.sh` default DMG URL is `https://persistent.oaistatic.com/codex-app-prod/Codex.dmg` and validates HTTPS.
- `scripts/lib/node-runtime.sh` uses Node `v22.22.2` with hard-coded SHA256 per architecture.
- `scripts/lib/native-modules.sh` downloads Electron from GitHub and uses npm with `--ignore-scripts` for initial install; build tooling still compiles native modules locally.
- `launcher/webview-server.py` binds to `127.0.0.1` by default.

## Audit decision

Proceeding to local proof is safe under the allowlist above because the selected path is non-root, local-checkout scoped, and avoids native package install/updater/service paths. Missing dependencies or build failures will stop the proof and be reported exactly; no dependency installation will be attempted.
