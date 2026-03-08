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

## Presets

### `default`

Base configuration for all repositories.

- Extends `config:best-practices` (Docker pinning, lockfile maintenance, abandoned package alerts)
- `minimumReleaseAge`: 7 days
- `rangeStrategy`: pin
- `schedule`: Sunday night (reduce PR noise)
- Automerge: minor and patch updates
- Major updates: labeled `breaking`, no automerge
- PR limits: 10 concurrent, 4/hour

### `node`

Extends `default` with Node.js rules:

- Dev dependencies: grouped + automerge
- ESLint ecosystem: grouped into single PR
- `@types/*`: grouped + automerge

### `go`

Extends `default` with Go rules:

- `go mod tidy` after dependency updates

### `php`

Extends `default` with PHP rules:

- `rangeStrategy`: update-lockfile (respect composer.json ranges)

### `terraform`

Extends `default` with Terraform rules:

- Automerge disabled (infrastructure changes require review)

## Ecosystem

| Repo | Role |
|---|---|
| [h13/dotfiles](https://github.com/h13/dotfiles) | Dev environment + `repo-init` (generates renovate.json) |
| [h13/.github](https://github.com/h13/.github) | Reusable CI workflows |
| [h13/renovate-config](https://github.com/h13/renovate-config) | Shared Renovate presets (this repo) |
