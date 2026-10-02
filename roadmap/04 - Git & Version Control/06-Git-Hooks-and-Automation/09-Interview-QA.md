# 09 - Git Hooks: Interview Questions & Answers

### Q1: Can client-side Git hooks be relied upon for organizational security compliance?
**Answer:** No. Client-side hooks execute locally on developer workstations and can be easily bypassed using `git commit --no-verify` or by disabling hooks. True compliance and security checks must be enforced server-side using GitHub/GitLab CI pipelines and server-side `pre-receive` hooks.

### Q2: What is the purpose of the `commit-msg` hook?
**Answer:** The `commit-msg` hook takes the path to a temporary file containing the commit message as an argument. It inspects the message before the commit is finalized and aborts (exit non-zero) if the message fails organizational standards (e.g. missing Jira issue ticket or violating Conventional Commits format).

### Q3: How does the `pre-commit` framework distribute hooks across a development team?
**Answer:** By storing hook definitions in a version-controlled `.pre-commit-config.yaml` file. Developers run `pre-commit install` once, and the framework automatically manages sandbox virtual environments (Python, Node, Go, Rust) for each hook.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
