# 08 - Git Hooks: Troubleshooting Guide

## 1. Hook Not Executing (Permission Denied)

On Linux/macOS, hook scripts must have execute permissions:
```bash
chmod +x .git/hooks/pre-commit
```

---

## 2. Cleaning Corrupted `pre-commit` Environments

If `pre-commit` throws environment caching errors after a Python or Node.js upgrade:
```bash
# Clean pre-commit cache
pre-commit clean

# Reinstall hook environments
pre-commit install-hooks
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
