# 02 - Forking Workflow vs. Shared Branch Model

## 1. Comparison Matrix

| Dimension | Forking Workflow | Shared Branch Model |
|---|---|---|
| **Repository Topology** | Each developer has a server-side clone (fork) | All developers push branches to a single shared repository |
| **Write Access** | Developers have **zero write access** to the upstream repo | Developers have branch push access to the shared repo |
| **Typical Use Case** | **Open Source**, Contractor/Vendor teams | **Internal Corporate Teams** |
| **PR Mechanism** | Pull Request submitted across repositories (Fork -> Upstream) | Pull Request submitted between branches in same repository |

---

## 2. Managing Upstream Remotes in a Fork

```bash
# View existing remotes
git remote -v

# Add upstream reference to original repository
git remote add upstream https://github.com/original-org/project.git

# Sync fork with latest upstream changes
git fetch upstream
git checkout main
git merge upstream/main
git push origin main
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Remotes Architecture](./01-Remotes-and-Tracking-Branches-Architecture.md) | [README](./README.md) | [03 - PR & MR Reviews](./03-Pull-Requests-and-Merge-Requests-Review-Excellence.md) |
