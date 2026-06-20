# Classification taxonomy

## Verdicts

- `PASS`: Linux wrapper behavior matches a documented official behavior oracle or actual supported-platform evidence for the tested scenario, and cleanup evidence exists.
- `PARTIAL`: Some behavior works, but the row lacks complete scope, macOS reference evidence, cleanup evidence, or a documented sub-feature remains untested.
- `FAIL`: A documented officially supported feature fails in a comparable supported-platform or complete docs-oracle scenario. Use sparingly; Linux-only absence usually is not `FAIL`.
- `NOT SUPPORTED`: Official docs or current UI/tool exposure show the feature is not available on Linux wrapper, or the feature is platform-specific to macOS/Windows and the wrapper does not expose a safe Linux equivalent.
- `ENTITLEMENT/ENV BLOCKED`: The test cannot run because account plan, workspace policy, region, installed plugin, browser availability, GitHub/`gh`, network mode, or local environment prerequisites are absent.
- `GATED`: The feature is intentionally not tested because it would cross a denied or unapproved gate.
- `UNTESTED`: In scope for evidence, but no current run proof exists yet.
- `WRAPPER DEFECT`: Evidence indicates the wrapper itself introduces a Linux-specific breakage or security caveat relative to official app expectations.

## Oracle hierarchy

1. Actual macOS reference evidence on a supported host with version/account/workspace recorded.
2. Official docs that fully specify expected behavior for macOS/Windows and document platform exceptions.
3. Existing Linux wrapper evidence only for Linux candidate status, never by itself for parity.

`Identical` is not an allowed verdict. Do not claim parity unless the row has either actual supported-platform evidence or a complete official behavior oracle.

## Evidence quality levels

- `A`: Screenshot/video plus log/transcript plus version pin plus cleanup proof.
- `B`: Log/transcript plus version pin plus cleanup proof.
- `C`: Inspect-only artifact or existing evidence import; useful for seeding but not enough for final parity.
- `D`: Docs-only oracle or explicit gate/blocked record.

## Status wording rules

- Say `Linux wrapper status`, not `Linux support`, unless citing official Linux support.
- For Computer Use, use `disabled/not exposed` and `NOT SUPPORTED` on Linux unless a separate approved helper/install plan changes the premise.
- For Chrome extension, distinguish native-host manifest/config presence from live `Connected` status.
- For web/browser rows, distinguish in-app browser, Browser Use, Developer Mode CDP, and Chrome extension; they have different gates.
- For security rows, report caveats directly even when they are not fixable in QA.
