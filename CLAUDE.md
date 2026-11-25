# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Shared Renovate configuration presets for h13 repositories. Consumers reference
presets remotely as `github>h13/renovate-config` (= `default.json`) or
`github>h13/renovate-config:<name>` (e.g. `:node`). Because resolution is
remote, **changes only affect consumers once merged to `main`** — there is no
local testing against a consuming repo short of pointing it at a branch
(`github>h13/renovate-config:node#branch-name`).

This is a configuration repo with no test suite by design; the quality bar is
the green Validate pipeline plus reading the diff.

## Commands

Validation (mirrors `.github/workflows/validate.yml`):

```sh
# Validate all presets against the Renovate schema (keep the version in sync
# with RENOVATE_VERSION in the workflow). If the local Node version does not
# satisfy renovate's engines field, prefix with npm_config_engine_strict=false
npx --yes --package renovate@43 -- renovate-config-validator *.json

# Format check / fix (oxfmt defaults, no local config)
oxfmt --check .
oxfmt .

# Markdown lint
markdownlint-cli2 '**/*.md'
```

CI additionally runs actionlint and typos on the workflow files.

## Architecture

- `default.json` — base preset. Extends `config:best-practices`, sets the
  Sunday-night JST schedule, 7-day `minimumReleaseAge`, PR limits, and the
  global automerge rules: patch/pin/digest always, minor only for current
  versions >= 1.0.0 (a 0.x minor can be breaking per semver). Majors get the
  `breaking` label and are never automerged.
- Stack presets (`node.json`, `go.json`, `php.json`, `github-actions.json`,
  `opentofu.json`) each extend
  `"github>h13/renovate-config"` — the **remote** default preset, not the local
  file.
- `renovate.json` — this repo's own Renovate config (dogfoods the default
  preset).
- `opentofu.json` is the IaC preset (there is no terraform preset; all h13
  IaC is OpenTofu, whose files Renovate's `terraform` manager handles, so
  `matchManagers: ["terraform"]` stays). Its provider rules must match
  depTypes `provider` AND `required_provider` — the latter is what a
  `required_providers` block (the standard modern declaration) produces.

### Rule-ordering semantics (the main thing to get right)

`packageRules` merge in `extends` order and apply sequentially — **last match
wins per setting**. Stack-preset rules therefore override `default.json`'s
automerge rule for the packages they match (e.g. `enabled: false` groups in
`node.json`, `automerge: false` for Terraform providers).

Security-update overrides must go in the top-level `vulnerabilityAlerts`
object, **not** in `packageRules` — a past bug (#46) applied vulnerability
settings to every update by putting them in a package rule.

### node.json's dev-dependencies group

The "dev dependencies" catch-all rule matches every devDependency and relies
on last-match-wins: the specific group rules after it (oxc, testing, types,
bundler, …) override its `groupName`. It must stay the **first** rule in
`node.json`. Keep bundler patterns from swallowing lookalike names (`vite`
is exact + `/^vite-/` precisely so vitest stays in the testing group).

### Custom manager

`default.json` defines a regex manager that updates values annotated with
`# renovate: datasource=... depName=...` comments in `.github/workflows/*.yml`
(used for reusable-workflow input defaults in consuming repos).

## Conventions

- Every preset keeps `$schema` and a `description` array as its first keys.
- README's Presets section documents each preset's behaviour — keep it in sync
  when changing any JSON file.
- Third-party actions in workflows are pinned to commit SHAs with the version
  in a trailing comment.
