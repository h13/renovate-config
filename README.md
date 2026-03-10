# renovate-config

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Shared [Renovate](https://docs.renovatebot.com/) configuration presets for h13 repositories.

## Usage

Add to your `renovate.json`:

```json
{
  "extends": ["github>h13/renovate-config"]
}
```

Or use a stack-specific preset:

```json
{
  "extends": ["github>h13/renovate-config:node"]
}
```

Combine multiple presets for projects with mixed stacks:

```json
{
  "extends": [
    "github>h13/renovate-config:node",
    "github>h13/renovate-config:github-actions"
  ]
}
```

## Presets

### `default`

Base configuration for all repositories.

- Extends `config:best-practices`
  (Docker pinning, lockfile maintenance, abandoned package alerts)
- `minimumReleaseAge`: 7 days
- `rangeStrategy`: pin (inherited from `config:best-practices`)
- `schedule`: Sunday 21:00 - Monday 06:00 JST (reduce PR noise)
- Automerge: minor and patch updates
- Major updates: labeled `breaking`, no automerge
- Security alerts: bypass waiting period and automerge immediately
- PR limits: 10 concurrent, 4/hour

### `node`

Extends `default` with Node.js rules:

- Dev dependencies: grouped into single PR
- ESLint ecosystem: grouped into single PR
- Prettier/Biome: grouped into single PR
- Testing (jest, vitest, mocha, chai, sinon, c8, nyc): grouped into single PR
- `@types/*`: grouped into single PR

### `go`

Extends `default` with Go rules:

- `go mod tidy` after dependency updates

### `php`

Extends `default` with PHP rules:

- `rangeStrategy`: update-lockfile (respect composer.json ranges)
- PHPStan: grouped into single PR
- PHPUnit: grouped into single PR
- Code style tools (php-cs-fixer, etc.): grouped into single PR

### `github-actions`

Extends `default` with GitHub Actions rules:

- Minor/patch updates: grouped into single PR
- Major updates: individual PR, no automerge

### `terraform`

Extends `default` with Terraform rules:

- Providers: automerge disabled (infrastructure changes require review)
- Modules: automerge minor/patch, require review for major

## Ecosystem

| Repo                                                          | Role                                                    |
| ------------------------------------------------------------- | ------------------------------------------------------- |
| [h13/dotfiles](https://github.com/h13/dotfiles)               | Dev environment + `repo-init` (generates renovate.json) |
| [h13/.github](https://github.com/h13/.github)                 | Reusable CI workflows                                   |
| [h13/renovate-config](https://github.com/h13/renovate-config) | Shared Renovate presets (this repo)                     |
