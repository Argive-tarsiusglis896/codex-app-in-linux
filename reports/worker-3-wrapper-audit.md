# worker-3 Wrapper Audit

## Scope

- Team task: `task-3`, owner `worker-3`.
- Wrapper repo: `<wrapper-source-url>`.
- Pinned checkout commit: `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`.
- Audited files: `Makefile`, `install.sh`, `scripts/install-deps.sh`, `scripts/lib/*.sh`, `launcher/start.sh.template`, `launcher/webview-server.py`, packaging hooks, user-local installer, Linux feature docs/manifests, updater docs.
- Syntax check: `bash -n install.sh scripts/install-deps.sh scripts/lib/install-helpers.sh scripts/lib/dmg.sh scripts/lib/node-runtime.sh scripts/lib/native-modules.sh scripts/lib/asar-patch.sh scripts/lib/webview-install.sh scripts/lib/bundled-plugins.sh scripts/lib/linux-features.sh launcher/start.sh.template scripts/build-deb.sh scripts/build-rpm.sh scripts/build-pacman.sh scripts/build-appimage.sh scripts/rebuild-candidate.sh` passed.

## First-proof allowlist

Only these actions are approved for this worker's local proof:

1. Read-only inspection of wrapper files and docs.
2. `git clone <wrapper-source-url> evidence/codex-linux-wrapper` and `git rev-parse HEAD` for a pinned checkout.
3. Dependency preflight using already-installed tools only.
4. Download the upstream DMG to `evidence/upstream/Codex.dmg` and record SHA256/size.
5. Local generated-app build from the frozen DMG only: `make build-app DMG=<absolute evidence/upstream/Codex.dmg>`.
6. Local generated-app launch only if build succeeds: `make run-app` or `./codex-app/start.sh`.
7. Log/screenshot capture under `evidence/` and reports under `reports/`.

## Denylist / separate-approval commands

Do not run these in the first proof:

- `sudo`, `pkexec`, package-manager install/update commands (`apt`, `apt-get`, `dnf`, `dnf5`, `pacman`, `zypper`, `rpm`, `dpkg -i`).
- `make bootstrap-native`, `make install-native`, `make update-native`, `make install`, `make package`, `make deb`, `make rpm`, `make pacman`, `make service-enable`, `make service-status`.
- `git pull --ff-only` or any in-place update path after pinning.
- `bash scripts/install-deps.sh` because it can run package managers, NodeSource setup, rustup, user/system 7zz bootstrap, and GUI prompt package install.
- User-local installer/update paths: `contrib/user-local-install/install-user-local.sh`, `codex-desktop-update`, and systemd timer enablement.
- Optional Linux feature enablement or feature staging outside defaults, especially remote/mobile control, Computer Use backend setup, browser native messaging extension setup, read-aloud runtime/model setup, agent-workspace skill install, and updater feature.
- Native package updater enablement or rollback/update manager operations.

## Security observations

- `Makefile` local proof targets `build-app`, `build-app-fresh`, and `run-app` route through `install.sh` and `codex-app/start.sh`; package targets and install targets are separate and denied.
- `install.sh` downloads/uses a DMG, installs a managed Node runtime inside the generated app, patches ASAR/webview assets, downloads Linux Electron, stages bundled plugin resources, and writes `codex-app/start.sh`.
- Default upstream DMG URL is `https://persistent.oaistatic.com/codex-app-prod/Codex.dmg` in `scripts/lib/dmg.sh`.
- Launcher webview server binds to `127.0.0.1` by default (`launcher/webview-server.py`, `launcher/start.sh.template`), not `0.0.0.0`.
- Native packages default to updater support (`PACKAGE_WITH_UPDATER ?= 1`) and install/update-manager service material; this is denied for first proof.
- User-local installer is still mutating: it writes `~/.local/bin`, `~/.local/opt`, desktop entries, state files, and can enable a systemd user timer. Denied.
- `scripts/install-deps.sh` is explicitly unsafe for this run because it uses sudo package managers, NodeSource repo setup, rustup, optional user/system 7zz bootstrap, and GUI prompt package installs.
- Optional Linux feature surface includes Chrome native messaging host, Computer Use backend/UI, remote/mobile control, read-aloud runtimes, agent workspace skill install, updater integration, and desktop/package hooks. No optional feature was enabled by worker-3.

## Audit verdict

The wrapper can be tested only via the local generated-app path. The first proof must stop if the frozen DMG cannot be extracted with already-present tooling. No package install, service enablement, updater path, system dependency install, or optional feature enablement is safe under this task without separate approval.
