# Codex App Linux team context

## Task statement
Execute the approved plan to validate whether Codex Desktop GUI can actually open on Linux through an unofficial wrapper.

## Desired outcome
Produce evidence-backed result: GUI launch success/failure, logs/screenshots if possible, audit notes, command allowlist/denylist, fallback recommendation, and a committed evidence report in this control repository.

## Known facts/evidence
- Official Codex desktop app is macOS/Windows only.
- Official Codex CLI supports Linux.
- Official Codex App can connect to Linux over SSH from Mac/Windows.
- Primary workaround: <wrapper-source-url>, converting upstream macOS Codex.dmg into a Linux Electron app.
- Secondary workaround: https://github.com/agustif/codex-linux.
- Wine is not preferred.
- Ralplan artifacts: /home/yun/tmp/.gjc/plans/ralplan/2026-06-18-0625-a191/stage-01-planner.md, stage-01-architect.md, stage-01-critic.md.

## Constraints
- First proof must avoid system-wide install.
- Do not run sudo/pkexec/systemctl/package install/bootstrap-native/install-native/install/update-native/setup-native without explicit leader approval.
- Prefer audit/read-only inspection first, then local build/run only after audit.
- Commit evidence report and any local scripts/checklists to this control repo.

## Unknowns/open questions
- Whether dependencies already exist for local build.
- Whether upstream DMG download succeeds.
- Whether Electron GUI can display in this Linux session.
- Whether login can complete in this environment.

## Likely touchpoints
- Wrapper repo checkout under this workspace.
- Evidence report under reports/.
- Screenshots/log copies under evidence/ if created.
