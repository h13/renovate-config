# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) configuration presets for h13 repositories.

## Presets

### `default`

Base configuration for all repositories.

- `config:recommended` + `helpers:pinGithubActionDigests`
- `minimumReleaseAge`: 7 days
- `rangeStrategy`: pin
- Automerge: minor and patch updates

```json
{
  "extends": ["github>h13/renovate-config"]
}
```

### `terraform`

Extends `default` with Terraform-specific rules (disables automerge for Terraform).

```json
{
  "extends": ["github>h13/renovate-config:terraform"]
}
```

## Ecosystem

See [h13/.github](https://github.com/h13/.github#ecosystem) for the full infrastructure diagram.

| リポ | 役割 |
|---|---|
| [h13/dotfiles](https://github.com/h13/dotfiles) | 開発環境 + `repo-init.sh` |
| [h13/renovate-config](https://github.com/h13/renovate-config) | Renovate 共有プリセット ★ this repo |
| [h13/.github](https://github.com/h13/.github) | CI テンプレート + SECURITY.md |
