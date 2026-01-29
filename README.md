# Project Name

[![CI](../../actions/workflows/ci.yml/badge.svg)](../../actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Brief description of your project.

## Features

- Feature 1
- Feature 2

## Installation

```bash
# Add installation instructions
```

## Usage

```bash
# Add usage examples
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT License - see [LICENSE](LICENSE) for details.

---

# Template Usage Guide

> **Delete this section after creating your repository.**

This is a GitHub template repository with pre-configured tooling for professional projects.

## What's Included

| File/Directory | Description |
|----------------|-------------|
| `.github/ISSUE_TEMPLATE/` | Bug report and feature request templates |
| `.github/PULL_REQUEST_TEMPLATE.md` | Pull request template with checklist |
| `.github/workflows/commitlint.yml` | Conventional commits validation |
| `.github/dependabot.yml` | Automated GitHub Actions updates |
| `.coderabbit.yaml` | AI code review configuration |
| `CONTRIBUTING.md` | Contribution guidelines |
| `SECURITY.md` | Security policy |
| `LICENSE` | MIT License (update year and name) |
| `.gitignore` | Common OS/editor ignores |

## Setup Checklist

After creating a repository from this template:

- [ ] Update `README.md` with your project info (delete this section)
- [ ] Update `LICENSE` with your name and year
- [ ] Update `SECURITY.md` with supported versions
- [ ] Enable CodeRabbit at [coderabbit.ai](https://coderabbit.ai)
- [ ] Enable GitHub branch protection rules
- [ ] Add language-specific CI workflow (see below)
- [ ] Add language-specific `.gitignore` entries

## Adding CI Workflows

This template includes only commit linting (language-agnostic). Add a `ci.yml` workflow for your stack:

### Workflow Structure

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      # Add linting steps

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      # Add test steps

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      # Add build steps
```

### Examples by Language

<details>
<summary><strong>Rust</strong></summary>

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: dtolnay/rust-toolchain@stable
        with:
          components: rustfmt, clippy
      - run: cargo fmt --check
      - run: cargo clippy -- -D warnings

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: cargo test

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: cargo build --release
```

Add to `.gitignore`:
```
/target/
Cargo.lock  # for libraries only
```

</details>

<details>
<summary><strong>Node.js / TypeScript</strong></summary>

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run format:check  # if using prettier

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
```

Add to `.gitignore`:
```
node_modules/
dist/
.next/
```

Add to `dependabot.yml`:
```yaml
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    commit-message:
      prefix: "chore(deps)"
```

</details>

<details>
<summary><strong>Python</strong></summary>

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install ruff
      - run: ruff check .
      - run: ruff format --check .

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install -e ".[test]"
      - run: pytest

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install build
      - run: python -m build
```

Add to `.gitignore`:
```
__pycache__/
*.py[cod]
.venv/
venv/
dist/
*.egg-info/
```

Add to `dependabot.yml`:
```yaml
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
    commit-message:
      prefix: "chore(deps)"
```

</details>

<details>
<summary><strong>Go</strong></summary>

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.22'
      - uses: golangci/golangci-lint-action@v4

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.22'
      - run: go test ./...

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.22'
      - run: go build ./...
```

Add to `.gitignore`:
```
/bin/
/dist/
```

Add to `dependabot.yml`:
```yaml
  - package-ecosystem: "gomod"
    directory: "/"
    schedule:
      interval: "weekly"
    commit-message:
      prefix: "chore(deps)"
```

</details>

## Syncing Template Updates

To keep repositories in sync with template updates, use [repo-file-sync-action](https://github.com/BetaHuhn/repo-file-sync-action):

1. Create a `.github/sync.yml` in this template repository:

```yaml
group:
  - files:
      - source: .github/ISSUE_TEMPLATE/
        dest: .github/ISSUE_TEMPLATE/
      - source: .github/PULL_REQUEST_TEMPLATE.md
        dest: .github/PULL_REQUEST_TEMPLATE.md
      - source: .github/dependabot.yml
        dest: .github/dependabot.yml
      - source: .coderabbit.yaml
        dest: .coderabbit.yaml
    repos: |
      owner/repo-1
      owner/repo-2
```

2. Create a workflow `.github/workflows/sync.yml`:

```yaml
name: Sync Files

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: BetaHuhn/repo-file-sync-action@v1
        with:
          GH_PAT: ${{ secrets.GH_PAT }}
```

3. Create a Personal Access Token with `repo` scope and add as `GH_PAT` secret

The action will create PRs in target repositories when files change.
