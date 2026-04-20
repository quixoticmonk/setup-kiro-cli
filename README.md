# setup-kiro-cli

A GitHub composite action that installs [kiro-cli](https://kiro.dev/docs/cli/) on a Linux runner and puts it on `PATH`. Like `actions/setup-node` but for `kiro-cli`.

## Usage

```yaml
- uses: quixoticmonk/setup-kiro-cli@v1
  with:
    cli-version: latest   # or a specific version like "2.0.1"

- run: kiro-cli chat --no-interactive "Summarize CHANGELOG.md"
  env:
    KIRO_API_KEY: ${{ secrets.KIRO_API_KEY }}
```

## Inputs

| Name | Default | Description |
|---|---|---|
| `cli-version` | `latest` | `latest` or an explicit version (e.g. `2.0.1`). |

## Outputs

| Name | Description |
|---|---|
| `cli-version` | Installed kiro-cli version. |

## Platform support

Linux runners only (`ubuntu-*`). Both `x86_64` and `aarch64`. glibc <2.34 auto-selects the `-musl` build.

## Authentication

This action does not authenticate for you. For CI, set `KIRO_API_KEY` as a secret and expose it as an env var when you invoke `kiro-cli`. Browser-based SSO is not usable on hosted runners.

## Version notes

Versioned URLs follow `https://desktop-release.q.us-east-1.amazonaws.com/{version}/kirocli-{arch}-linux.zip`. Not every historical version is hosted; if the requested version 404s, the step fails with the URL so you can verify.
