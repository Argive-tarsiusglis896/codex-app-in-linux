# worker-3 Local Proof Report

## Status

Blocked after safe audit and frozen-artifact build attempt. The local GUI did not launch because the frozen upstream DMG could not be extracted by already-present tooling. Per task rules, no dependency bootstrap, package-manager command, system install, service enablement, updater path, or optional feature enablement was run.

## Environment evidence

- OS: Ubuntu 24.04.4 LTS (`/etc/os-release`).
- Kernel/arch: `Linux yun-Zenbook-UM3402YA-UM3402YA 6.17.0-35-generic #35~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Tue May 26 19:30:42 UTC 2 x86_64 GNU/Linux`.
- Session: `XDG_SESSION_TYPE=wayland`, `XDG_CURRENT_DESKTOP=ubuntu:GNOME`, `DISPLAY=:0`, `WAYLAND_DISPLAY=wayland-0`.
- Codex CLI baseline: `/home/yun/.npm-global/bin/codex`, `codex-cli 0.136.0`.
- Wrapper checkout: `evidence/codex-linux-wrapper`, commit `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`.

## Upstream artifact evidence

- URL: `https://persistent.oaistatic.com/codex-app-prod/Codex.dmg`.
- Frozen file: `evidence/upstream/Codex.dmg`.
- SHA256: `ae7b81f935b7babf953c22400d23c503215457ff6892e98aba3c48ffab67c54e`.
- Size: `74409756` bytes.
- Metadata file: `evidence/upstream/download-info.txt`.

## Dependency preflight

Present:

- `python3=/usr/bin/python3` (`Python 3.12.3`)
- `curl=/usr/bin/curl` (`curl 8.5.0`)
- `unzip=/usr/bin/unzip` (`UnZip 6.00`)
- `tar=/usr/bin/tar`
- `make=/usr/bin/make` (`GNU Make 4.3`)
- `g++=/usr/bin/g++` (`g++ 13.3.0`)
- `cargo=/home/yun/.cargo/bin/cargo` (`cargo 1.94.0`)
- `codex=/home/yun/.npm-global/bin/codex` (`codex-cli 0.136.0`)
- `7zip=/usr/bin/7z` (`7-Zip 23.01`)

Effective missing/blocked dependency:

- APFS/current-DMG-capable `7zz` or equivalent extractor. `/usr/bin/7z` 23.01 exists, but `make build-app` failed with `Open ERROR: Cannot open the file as [Dmg] archive` against the frozen DMG. `scripts/install-deps.sh` could bootstrap a newer 7zz, but that path is denied because it can run package managers/rustup/user-system bootstrap; the task says stop and report missing deps.

## Commands run

Allowed and completed:

- `git clone <wrapper-source-url> evidence/codex-linux-wrapper`
- `git rev-parse HEAD` -> `9125911c8347c35177dfc76e2f5bce2b8b2e41d4`
- Dependency preflight/version checks.
- Script syntax check: passed, see `reports/worker-3-wrapper-audit.md`.
- Frozen DMG download and checksum capture.

Allowed but failed:

- From `evidence/codex-linux-wrapper`: `make build-app DMG="$PWD/../upstream/Codex.dmg"`
- Exit: `2`
- Failure evidence: `evidence/logs/build-app-failure-observed.txt`
- Build stdout log: `evidence/logs/build-app.log`

Not run:

- `make run-app` / `./codex-app/start.sh`, because no complete app was generated.
- GUI auth/project/thread/shell QA, because launch was unavailable.
- Screenshots, because no GUI launched. Screenshot path remains `evidence/screenshots/` with no captures.

## Manual QA checklist for the next unblocked run

1. Install/provide approved APFS-capable `7zz` or equivalent without violating system-mutation rules.
2. Re-run `make build-app DMG=<absolute evidence/upstream/Codex.dmg>` from the pinned checkout.
3. Verify generated files: `codex-app/start.sh`, `codex-app/.codex-linux/patch-report.json`, build info tying the app to SHA256 `ae7b81f935b7babf953c22400d23c503215457ff6892e98aba3c48ffab67c54e`.
4. Launch with `make run-app` or `./codex-app/start.sh`; capture `~/.cache/codex-desktop/launcher.log` and a screenshot under `evidence/screenshots/`.
5. Confirm window reaches usable signed-in or sign-in-ready state.
6. Open a disposable project only.
7. Start a harmless thread and record either response or expected auth/approval flow.
8. Verify approval controls before any shell/file action.
9. Run a harmless command/file write scoped to the disposable project only.
10. Confirm webview listener remains loopback-only and no public/shared app-server transport is exposed.
11. Record rollback notes for generated app/cache cleanup.

## Fallback recommendation

Use the official supported Codex App on macOS/Windows connected to this Linux host over SSH as the safety fallback if local GUI proof remains blocked by extractor/build/security/auth issues. Option C (`agustif/codex-linux`) should only be separately reviewed after this Option A block is accepted. Wine remains rejected unless explicitly approved after better paths fail.

## Rollback / cleanup notes

No system install or service was created. The failed build left local workspace artifacts only:

- `evidence/codex-linux-wrapper/codex-app/resources/node-runtime/` partial generated-app content.
- `evidence/upstream/Codex.dmg` frozen artifact.
- Logs and reports under `evidence/` and `reports/`.

Cleanup, if desired after evidence collection: remove `evidence/codex-linux-wrapper/codex-app/`. Do not remove reports or frozen DMG until integration completes.
