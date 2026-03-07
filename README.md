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
