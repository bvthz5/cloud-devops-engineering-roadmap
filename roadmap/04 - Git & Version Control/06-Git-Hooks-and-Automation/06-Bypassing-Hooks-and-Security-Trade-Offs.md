# 06 - Bypassing Hooks and Security Trade-Offs

## 1. The `--no-verify` Flag

Any developer can bypass all client-side pre-commit and commit-msg hooks by adding `-n` or `--no-verify`:
```bash
git commit -m "bypass checks" --no-verify
git push --no-verify
```

---

## 2. Why Client Hooks Are Never a Substitute for Server CI

> **Golden Rule of DevOps Security:** Client-side hooks are a productivity convenience for the developer; Server-side CI checks and branch protection are the non-negotiable security boundary!

Never trust that client-side hooks ran on developer workstations. Always re-run linters, tests, and secret scans inside your server CI pipeline.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Server-Side Hooks](./05-Server-Side-Hooks-pre-receive-and-post-receive.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
