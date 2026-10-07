# Run MIR GitHub Action

Runs [MIR](https://github.com/jorgsowa/mir), a PHP static analyzer, in a GitHub Actions workflow.

## Usage

Create `.github/workflows/mir.yml` in your PHP project:

```yaml
name: MIR

on:
  pull_request:
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7.0.1
      - uses: jorgsowa/mir-action@v1
        with:
          path: src
```

The action installs MIR `0.86.0` by default. For reproducible builds, pin the action to a commit SHA; the `version` input must be an exact MIR package version.

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `path` | `.` | PHP file or directory to analyze. |
| `version` | `0.86.0` | Exact MIR Cargo crate version to install. |
| `php-version` | `8.5` | PHP language version MIR should target. |
| `arguments` | `[]` | Extra CLI arguments as a JSON array of strings. |

For example, to request JSON output without exposing workflow inputs to shell evaluation:

```yaml
- uses: jorgsowa/mir-action@v1
  with:
    path: src
    version: '1.2.3'
    arguments: '["--format", "json"]'
```

The action fails the job when MIR reports diagnostics or cannot run. It exposes the installed MIR version as `steps.<id>.outputs.version`.
