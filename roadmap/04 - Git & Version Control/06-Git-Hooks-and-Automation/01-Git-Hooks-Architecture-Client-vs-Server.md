# 01 - Git Hooks Architecture: Client vs. Server

## 1. What are Git Hooks?

Git hooks are custom scripts triggered automatically when specific events occur in the Git lifecycle.
- Stored locally in: `.git/hooks/`
- Standard hook scripts are written in **Bash**, **Python**, or **Go**.
- **Exit Code Rule:** If a hook exits with **status 0**, Git proceeds. If it exits with a **non-zero status**, Git immediately aborts the operation!

```
+-------------------------------------------------------------------------------+
|                             GIT HOOKS LIFECYCLE                               |
|                                                                               |
| [git commit] ──► pre-commit ──► prepare-commit-msg ──► commit-msg ──► Commit! |
|                                                                               |
| [git push]   ──► pre-push ──────────────────────────────────────────► Push!   |
|                                                                               |
| SERVER SIDE: ──► pre-receive ──► update ───────────► post-receive (Deploy!)   |
+-------------------------------------------------------------------------------+
```

---

## 2. Client-Side vs. Server-Side Hooks

- **Client-Side Hooks:** Run on developer laptops. Used for fast feedback (formatting with Prettier/Black, local linting, secret detection).
- **Server-Side Hooks:** Run on the Git server (GitHub Enterprise, GitLab, Gerrit). Used for ironclad enforcement (rejecting commits without Jira ticket numbers, blocking unsigned commits).

*Important:* `.git/hooks/` is **NOT committed or cloned**. To share hooks across a team, you must use a framework like `pre-commit` or configure `core.hooksPath`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-GitHub-and-GitLab-Collaboration)](../05-GitHub-and-GitLab-Collaboration/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Client Side Hooks pre commit commit msg pre push →](./02-Client-Side-Hooks-pre-commit-commit-msg-pre-push.md) |
