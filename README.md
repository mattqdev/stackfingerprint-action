<p align="center">
  <img src=".github/assets/logo.png" alt="Stack Fingerprint" width="120" />
</p>

<h1 align="center">Stack Fingerprint Action</h1>

Detect your repository's tech stack and commit an embeddable SVG card, generated on GitHub's own runners. Your README then serves a local file instead of hotlinking a third-party image.

[![Stack Fingerprint](https://stackfingerprint.vercel.app/api/card?repo=mattqdev/stackfingerprint)](https://stackfingerprint.vercel.app/?repo=mattqdev/stackfingerprint)

Design your card visually at **[stackfingerprint.vercel.app](https://stackfingerprint.vercel.app)**, then copy the options here.

## Usage

`.github/workflows/stack-fingerprint.yml`:

```yaml
name: Stack Fingerprint

on:
  push:
    branches: [main]
  schedule:
    - cron: "0 4 * * 1" # weekly refresh
  workflow_dispatch:

permissions:
  contents: write

jobs:
  card:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: mattqdev/stackfingerprint-action@v1
        with:
          theme: scanner
          layout: classic
```

Then add it to your README:

```markdown
[![Stack Fingerprint](./assets/stack-fingerprint.svg)](https://stackfingerprint.vercel.app/?repo=OWNER/REPO)
```

## Inputs

| Input            | Default                                           | Description                                                                 |
| ---------------- | ------------------------------------------------- | --------------------------------------------------------------------------- |
| `layout`         | `classic`                                         | `classic` `compact` `banner` `tall` `terminal` `minimal` `icons` `sidebar` `split` `cards` |
| `theme`          | `scanner`                                         | Any theme from the [builder](https://stackfingerprint.vercel.app)           |
| `icon-style`     | `color`                                           | `color` `mono` `none` `icononly`                                            |
| `size`           | `md`                                              | `sm` `md` `lg` `xl`                                                         |
| `filter`         | `all`                                             | `all` `top` `core` `devtools` `infra` `prodonly`                            |
| `path`           | —                                                 | Sub-directory to scan (monorepo), e.g. `apps/web`                           |
| `output`         | `assets/stack-fingerprint.svg`                    | Where the SVG is saved                                                      |
| `commit`         | `true`                                            | Commit and push the SVG when it changes                                     |
| `commit-message` | `chore: update stack fingerprint badge [skip ci]` | Commit message                                                              |
| `api-url`        | `https://stackfingerprint.vercel.app/api/card`    | Point at your own deployment to self-host                                   |

## Outputs

| Output     | Description                                   |
| ---------- | --------------------------------------------- |
| `svg-path` | Path of the generated SVG                     |
| `changed`  | `true` if the SVG differs from the last commit |

### Monorepo: one card per package

```yaml
      - uses: mattqdev/stackfingerprint-action@v1
        with: { path: apps/web, output: assets/web.svg, commit: false }
      - uses: mattqdev/stackfingerprint-action@v1
        with: { path: apps/api, output: assets/api.svg }
```

## License

MIT — part of [Stack Fingerprint](https://github.com/mattqdev/stackfingerprint).
