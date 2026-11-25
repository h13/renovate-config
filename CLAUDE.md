# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Shared Renovate presets, consumed remotely as `github>h13/renovate-config`
(= `default.json`) or `:<name>`. Because resolution is remote, **changes only
affect consumers once merged to `main`**.

## Quality bar

No test suite by design. The bar is the green Validate pipeline (see
`.github/workflows/validate.yml`) plus reading the diff. Locally:

```sh
# If local Node doesn't satisfy renovate's engines field,
# prefix with npm_config_engine_strict=false
npx --yes --package renovate@43 -- renovate-config-validator *.json
oxfmt --check .
markdownlint-cli2 '**/*.md'
```

## Non-obvious rules

- `packageRules` merge in `extends` order and apply **last-match-wins** per
  setting. Two consequences: the dev-dependencies catch-all in `node.json`
  must stay the first rule (the specific groups after it override its
  `groupName`), and automerge policy belongs in `default.json` only — a
  duplicated `automerge` in a stack preset silently overrides future policy
  changes.
- Security-update overrides go in the top-level `vulnerabilityAlerts` object,
  **not** in `packageRules` (a package rule applied them to every update — #46).
- `opentofu.json` is the IaC preset; Renovate's `terraform` manager handles
  OpenTofu files, so `matchManagers: ["terraform"]` stays. Provider rules must
  match depTypes `provider` AND `required_provider` (a `required_providers`
  block produces the latter).
- README documents only this repo's own decisions; anything inherited gets a
  link to the upstream doc, not a summary. Keep it in sync with the JSON.
