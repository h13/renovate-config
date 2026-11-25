# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Shared Renovate presets, consumed remotely as `github>h13/renovate-config`
(= `default.json`) or `:<name>`. Changes only affect consumers once merged
to `main`.

No test suite by design: the quality bar is the green Validate pipeline
(`.github/workflows/validate.yml`; run the same commands locally — if local
Node doesn't satisfy renovate's `engines`, prefix
`npm_config_engine_strict=false`) plus reading the diff.

Non-obvious rules:

- `packageRules` apply last-match-wins. The dev-dependencies catch-all in
  `node.json` stays first, and automerge policy lives in `default.json` only —
  an `automerge` duplicated into a preset silently overrides future policy
  changes.
- Security-update overrides go in `vulnerabilityAlerts`, not `packageRules`
  (#46).
- Provider rules need depTypes `provider` AND `required_provider`; Renovate's
  `terraform` manager handles OpenTofu files.
