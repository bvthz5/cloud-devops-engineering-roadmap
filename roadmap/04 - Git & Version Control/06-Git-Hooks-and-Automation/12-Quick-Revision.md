# 12 - Git Hooks: Quick Revision Cheat Sheet

## Hooks Lifecycle & Roles

| Hook Name | When it Runs | Typical Use Case |
|---|---|---|
| `pre-commit` | Before commit message editor opens | Linters, formatters, secret scanning |
| `commit-msg` | After message is written | Validating SemVer / Jira ticket patterns |
| `post-commit` | Immediately after commit is saved | Notifications, local metrics |
| `pre-push` | Before remote push initiates | Running fast integration test suite |
| `pre-receive` | On server before accepting push | Organizational branch policies & approvals |
| `post-receive` | On server after push is merged | Triggering automated deployment script |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [07 - Resolving Conflicts](../07-Resolving-Conflicts-and-Troubleshooting/README.md) |
