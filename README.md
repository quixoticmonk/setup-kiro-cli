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

## Publishing

1. **Create the repo** at `github.com/quixoticmonk/setup-kiro-cli` (public). The repo name must match the `uses:` reference.
2. **Push `action.yml` to the root.** The file must be at the repo root for GitHub to recognise it as an action.
3. **Tag a release.**
   ```bash
   git tag -a v1.0.0 -m "Initial release"
   git push origin v1.0.0
   ```
   Then create a GitHub Release from that tag (UI or `gh release create v1.0.0 --generate-notes`). When you create the release, check **"Publish this Action to the GitHub Marketplace"** and accept the terms. Marketplace requires a unique `name:` and a valid `branding:` block — both already set.
4. **Add a moving major tag** so users can pin to `@v1`:
   ```bash
   git tag -f v1 v1.0.0
   git push -f origin v1
   ```
   Update this tag on each non-breaking release. Users who pin to `@v1` get the latest 1.x automatically.
5. **Subsequent releases:** tag `vX.Y.Z`, re-point `vX`, publish a new Release. Breaking changes → bump major and publish as `v2`.

Users can consume the action before Marketplace listing — any public repo with an `action.yml` at root is usable via `uses: owner/repo@ref`. Marketplace only affects discoverability.
