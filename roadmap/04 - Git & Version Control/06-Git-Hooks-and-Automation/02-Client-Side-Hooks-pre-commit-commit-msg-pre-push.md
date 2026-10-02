# 02 - Client-Side Hooks: pre-commit, commit-msg, pre-push

## 1. `pre-commit` Hook

Runs before a commit is even created. Used to inspect the staged snapshot:

```bash
#!/usr/bin/env bash
# .git/hooks/pre-commit

echo "Running automated linter..."
if ! flake8 src/; then
    echo "ERROR: Flake8 linting failed. Aborting commit."
    exit 1
fi
exit 0
```

---

## 2. `commit-msg` Hook

Inspects the commit message written by the user. Perfect for enforcing Conventional Commits:

```bash
#!/usr/bin/env bash
# .git/hooks/commit-msg

COMMIT_MSG_FILE=$1
MSG=$(cat "$COMMIT_MSG_FILE")

# Enforce Conventional Commit prefix
if ! echo "$MSG" | grep -qE '^(feat|fix|docs|refactor|test|chore|ci)(\(.+\))?: .+'; then
    echo "ERROR: Invalid commit message format."
    echo "Expected format: <type>(scope): <description>"
    exit 1
fi
```

---

## 3. `pre-push` Hook
Runs during `git push`. Ideal for running fast integration tests to avoid polluting remote CI runners with broken code.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Git Hooks Architecture Client vs Server](./01-Git-Hooks-Architecture-Client-vs-Server.md) | [Index](../../../README.md) | [03 - The pre commit Framework Standardization →](./03-The-pre-commit-Framework-Standardization.md) |
