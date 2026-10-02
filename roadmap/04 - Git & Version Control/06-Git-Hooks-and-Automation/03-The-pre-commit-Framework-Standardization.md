# 03 - The pre-commit Framework Standardization

## 1. Why Use the `pre-commit` Framework?

Because `.git/hooks/` is ignored by Git, teams historically had no way to ensure all 50 developers ran identical hooks.
The **`pre-commit` framework** (Python-based) solves this by managing hook versions declaratively in a committed configuration file: `.pre-commit-config.yaml`.

---

## 2. Production `.pre-commit-config.yaml` Blueprint

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-json
      - id: check-merge-conflict
      - id: detect-private-key

  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.2
    hooks:
      - id: gitleaks

  - repo: https://github.com/psf/black
    rev: 24.2.0
    hooks:
      - id: black
```

```bash
# Install framework and activate hooks in current repo
pip install pre-commit
pre-commit install

# Manually execute all hooks across all files
pre-commit run --all-files
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Client-Side Hooks](./02-Client-Side-Hooks-pre-commit-commit-msg-pre-push.md) | [README](./README.md) | [04 - Secret Scanning](./04-Secret-Scanning-and-Credential-Leak-Prevention.md) |
