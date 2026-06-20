# Codex app Linux-wrapper mac-parity QA — 2026-06-20

Status: COMPLETE FOR SAFE QA SCOPE — team execution finished all three lanes. Verdict: PASS WITH LIMITATIONS for core local Linux wrapper flows; NOT FULL PARITY.

## Support boundary

Official Codex desktop app support is macOS and Windows. This report evaluates unofficial Linux-wrapper compatibility only and must not be quoted as OpenAI Linux desktop support.

## Run artifacts

- Manifest: `evidence/mac-parity-qa-20260620/run-manifest.md`
- Taxonomy: `evidence/mac-parity-qa-20260620/taxonomy.md`
- Gates: `evidence/mac-parity-qa-20260620/gates.md`
- Matrix CSV: `evidence/mac-parity-qa-20260620/feature-matrix.csv`
- Matrix MD: `evidence/mac-parity-qa-20260620/feature-matrix.md`
- Lane index: `evidence/mac-parity-qa-20260620/lane-evidence-index.md`
- Final verdict seed: `evidence/mac-parity-qa-20260620/final-verdict.md`
- Lane B report: `reports/mac-parity-qa-20260620-lane-b.md`
- Lane C report: `reports/mac-parity-qa-20260620-lane-c.md`

## Team lanes

- Lane A — Matrix/oracle/evidence marshal: seeded 38 feature/security rows, run manifest, taxonomy, gate ledger, and final report skeleton.
- Lane B — Linux local app core QA: fresh startup/webview checks plus current-log inspection and copied prior controlled smokes.
- Lane C — Integrations/security review: static/runtime integration classification, Chrome/native-host checks, Computer Use/Appshots classification, and security rows.

## Matrix summary

- PASS / safe evidence: startup/window/render, webview HTTP reachability, app-server stdio transport, existing account readiness in logs, basic chat prior smoke, harmless shell prior smoke, chat search prior smoke, Browser Use on public non-auth page, loopback webview binding, native privileged-install denylist, redaction discipline.
- PARTIAL: project/file dialog, multi-chat depth, Git/worktree/review UI, approval/sandbox boundaries beyond harmless command, plugins/skills discovery, Chrome native-host setup.
- NOT SUPPORTED: Computer Use as official parity on Linux. Official docs support macOS/Windows; Linux wrapper currently exposes no usable Computer Use tool in app QA.
- WRAPPER DEFECT / security caveat: Electron launches with `--no-sandbox` in Linux wrapper process evidence.
- GATED / UNTESTED / ENV BLOCKED: fresh clean login, settings restart persistence, full project workflow, Chrome extension Connected with disposable profile, Developer Mode CDP, MCP, Sites, IDE sync, web search, image input/generation, memories, automations, notifications, prevent sleep, remote control, Appshots, production push/PR, native install/updater/service/package.

## Lane B findings

Lane B reports PASS WITH LIMITATIONS for safe local Linux generated-app QA:

- Fresh startup check: local webview returned HTTP 200.
- Current launcher log showed webview origin verification, Electron spawn, stdio app-server transport, Codex CLI initialization, primary window ready-to-show, repeated `account/read` success, Browser Use IAB backend ready, and updater disabled.
- Existing just-now interactive evidence copied into this run showed controlled chat (`codex-linux-qa-ok`), harmless shell command (`codex-linux-shell-ok`), chat search, and Browser Use (`Example Domain`) success.
- Limitations: plain headless Chromium is not a representative Electron-host UI because the app depends on Electron host IPC; project file dialog, settings restart, and full Git/review UI were not end-to-end retested in this safe lane.

Evidence: `reports/mac-parity-qa-20260620-lane-b.md`, `evidence/mac-parity-qa-20260620/lane-b/`.

## Lane C findings

Lane C final review status is WATCH, not CLEAR:

- Browser Use on public non-auth pages: PASS based on prior smoke plus plugin presence.
- Chrome extension native host: PARTIAL/GATED. Chrome is installed/running, but disposable `HOME` had no native-host manifest and no extension Connected screen was produced; personal Chrome profile use remained denied.
- Computer Use: NOT SUPPORTED for official parity. Linux wrapper has inspectable Linux-alpha resources, but current app tool surface is disabled/not exposed and helper install was denied.
- Appshots/MCP/Sites/IDE sync/web search/images/memories/automations/notifications/prevent-sleep/remote-control: UNTESTED, GATED, or ENTITLEMENT/ENV BLOCKED.
- Webview loopback binding and app-server stdio transport: PASS.
- Electron `--no-sandbox`: WRAPPER DEFECT / SECURITY CAVEAT.

Evidence: `reports/mac-parity-qa-20260620-lane-c.md`, `evidence/mac-parity-qa-20260620/lane-c/`.

## Gates observed

No denied gate was approved or crossed. The run did not use `sudo`, `pkexec`, package-manager commands, `systemctl`, native install/updater/service/package commands, personal browser profile access, production remote push/PR, sensitive signed-in websites, Computer Use helper install, wrapper/source edits, commits, or pushes.

## Final verdict

The Linux wrapper demonstrates local generated-app viability for core safe flows and in-app Browser Use, but it does **not** establish official macOS/Windows feature parity. The correct classification is PASS WITH LIMITATIONS for core local workflows, WATCH for integration/security parity, and NOT FULL PARITY overall.

Recommended usage remains: use the local generated Linux wrapper for the core GUI workflows already covered by evidence; use official Codex app on macOS/Windows or Codex CLI/SSH for supported production-grade workflows, especially Computer Use, Chrome signed-in browser automation, automations, remote control, native desktop integration, and security-sensitive work.

## Required follow-ups before stronger parity claims

1. Actual macOS baseline run on the same account/app version, or keep macOS oracle docs-only.
2. Fresh clean login test.
3. Full project/file-dialog/settings restart flow.
4. Disposable Chrome profile with extension Connected proof.
5. MCP/plugin/Sites/IDE/web-search/image/memory/automation/notification/prevent-sleep/remote-control test fixtures.
6. Security decision on Electron `--no-sandbox` before any daily-driver claim.


## Ultragoal G002 remaining QA update

Additional safe remaining-scope QA was completed after the team run. Evidence: `reports/mac-parity-qa-20260620-remaining.md` and `evidence/mac-parity-qa-20260620/remaining/ui-surface-results.json`.

New results: Plugins catalog PASS; Automations UI PARTIAL; Add files/Agents menu PASS menu-only; Codex mobile page PARTIAL with Linux copy bug (`Connect your phone to this Mac`); web search PASS after final result returned; Project picker FAIL/not exercisable via CDP; Settings FAIL/not exercisable via CDP; Dictate present/gated; image/file input partial/gated. Overall verdict remains NOT FULL PARITY.


### G002 blocker-disposition addendum

After quality review, the remaining report now explicitly classifies the omitted G002 surfaces in `evidence/mac-parity-qa-20260620/remaining/blocker-dispositions.json`: fresh login/session persistence gated, Git/worktree/review/local commit environment-blocked by project picker failure, Chrome disposable profile partial/gated, MCP/memories/notifications/prevent-sleep gated, Appshots environment/platform-blocked, artifact preview blocked, and floating pop-out untested/no visible control.
