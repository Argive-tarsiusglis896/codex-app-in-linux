# mac-parity-qa-20260620 — Lane C integrations/security review

See canonical lane evidence at `evidence/mac-parity-qa-20260620/lane-c/integration-security-review.md`.

## Verdict

**WATCH**. Safe evidence supports Browser Use PASS, loopback webview PASS, app-server stdio PASS, native privileged install denylist PASS, and redaction PASS. It does **not** support a full Linux-wrapper parity declaration.

Material blockers/gaps:

- Chrome extension native-host live Connected proof is **PARTIAL/GATED**. Chrome is installed, but disposable `HOME` has no native-host manifest; existing running Chrome was not controlled because personal-profile provenance was not disposable.
- Computer Use is **NOT SUPPORTED for official parity**. Official docs state macOS/Windows support; Linux wrapper bundles an inspect-only Linux alpha plugin resource but runtime QA says the tool is disabled/not exposed.
- Electron launches with `--no-sandbox`, a material wrapper security caveat.
- MCP, Sites, IDE sync, web search, image input/generation, memories, automations, notifications, prevent-sleep, remote-control, and Appshots remain untested, gated, or environment-blocked.

Cross-lane inspection found Lane A's report skeleton still pending Lane B/C at inspection time; Lane B's report supports PASS WITH LIMITATIONS for safe local generated-app checks. That aligns with this lane's WATCH status rather than a parity-clear status.

## Evidence index

- `evidence/mac-parity-qa-20260620/lane-c/commands/command-results.json`
- `evidence/mac-parity-qa-20260620/lane-c/manifest-index.json`
- `evidence/mac-parity-qa-20260620/lane-c/manifests/`
- `evidence/mac-parity-qa-20260620/lane-c/commands/`

No wrapper/source code was edited by this lane.
