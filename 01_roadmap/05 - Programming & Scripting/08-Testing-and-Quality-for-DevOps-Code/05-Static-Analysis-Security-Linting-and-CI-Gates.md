# 05 - Static Analysis, Security Linting, and CI Gates

## 1. Enterprise Code Quality Linters

Static analysis catches vulnerabilities, style drift, and unoptimized logic before code is committed.

| Language | Static Analysis Tool | Primary Focus |
|---|---|---|
| **Bash** | `shellcheck` | Syntax hazards, quote escaping, unhandled exit codes. |
| **Python** | `ruff` (or `flake8` + `black`) | Ultra-fast Rust-based Python linter and code formatter. |
| **Python** | `bandit` | AST-based security vulnerability scanner (hardcoded credentials, SQL injection, insecure imports). |
| **PowerShell**| `PSScriptAnalyzer` | Best practice cmdlets, security hazards (`ConvertTo-SecureString -AsPlainText`). |
| **Go** | `golangci-lint` | Unified runner aggregating 50+ Go linters (`govet`, `errcheck`, `staticcheck`). |

---

## 2. Enforcing Pre-Commit Hooks (`.pre-commit-config.yaml`)

Pre-commit hooks intercept `git commit` commands locally, preventing bad code from ever reaching the remote repository.

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: check-yaml
      - id: check-json
      - id: end-of-file-fixer
      - id: trailing-whitespace
      - id: detect-private-key

  - repo: https://github.com/koalaman/shellcheck-precommit
    rev: v0.9.0
    hooks:
      - id: shellcheck

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.3.0
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format

  - repo: https://github.com/PyCQA/bandit
    rev: 1.7.7
    hooks:
      - id: bandit
        args: ["-c", "pyproject.toml", "-r", "scripts/"]
```

```bash
# Install and initialize
pip install pre-commit
pre-commit install
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Golang Testing and Testcontainers](./04-Golang-Testing-and-Testcontainers.md) | [Index](../../../README.md) | [06 - Mutation Testing and Resilience Validation →](./06-Mutation-Testing-and-Resilience-Validation.md) |
