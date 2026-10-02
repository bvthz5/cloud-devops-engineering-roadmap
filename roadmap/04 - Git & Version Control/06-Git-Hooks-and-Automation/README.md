# Module 06: Git Hooks and Client/Server Automation

Welcome to **Module 06: Git Hooks and Client/Server Automation**. Automated quality checks, code formatting, and secret leak prevention should occur before code ever leaves a developer's workstation. Git hooks provide the mechanism for local and server-side automation.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Differentiate between **Client-Side Hooks** (`pre-commit`, `commit-msg`, `pre-push`) and **Server-Side Hooks** (`pre-receive`, `post-receive`).
2. Write custom Bash and Python hook scripts in `.git/hooks/`.
3. Standardize and distribute multi-language hooks across engineering teams using the **`pre-commit`** framework.
4. Implement automated **Secret Scanning** (`gitleaks`, `trufflehog`) to prevent API keys and credentials from ever being committed.
5. Enforce Conventional Commit message standards using `commitlint` and `commit-msg` hooks.
6. Build lightweight automated deployment pipelines using server-side `post-receive` hooks.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Git Hooks Architecture](./01-Git-Hooks-Architecture-Client-vs-Server.md) | The lifecycle of hooks, non-executable defaults, exit code significance |
| 02 | [Client-Side Hooks Deep Dive](./02-Client-Side-Hooks-pre-commit-commit-msg-pre-push.md) | Linting, formatting, validating commit messages, blocking bad pushes |
| 03 | [The pre-commit Framework](./03-The-pre-commit-Framework-Standardization.md) | `.pre-commit-config.yaml`, automated environments, multi-language hooks |
| 04 | [Secret Scanning & Leak Prevention](./04-Secret-Scanning-and-Credential-Leak-Prevention.md) | Preventing AWS/GCP key leaks, `gitleaks`, `trufflehog`, pre-commit integration |
| 05 | [Server-Side Hooks & Gateways](./05-Server-Side-Hooks-pre-receive-and-post-receive.md) | Enterprise branch policy enforcement, automated Git deployment triggers |
| 06 | [Bypassing Hooks & Security Trade-Offs](./06-Bypassing-Hooks-and-Security-Trade-Offs.md) | The `--no-verify` flag, client hook limitations, why server gating is mandatory |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | $60,000 AWS bill from leaked credentials, broken pre-commit blocking hotfix |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Fixing `hook failed to execute`, permission issues, debugging pre-commit cache |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Writing an automated pre-commit secret scanner and commit message validator |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Hook execution lifecycle, exit code rules, pre-commit config template |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - GitHub & GitLab](../05-GitHub-and-GitLab-Collaboration/README.md) | [README](./README.md) | [01 - Hooks Architecture](./01-Git-Hooks-Architecture-Client-vs-Server.md) |
