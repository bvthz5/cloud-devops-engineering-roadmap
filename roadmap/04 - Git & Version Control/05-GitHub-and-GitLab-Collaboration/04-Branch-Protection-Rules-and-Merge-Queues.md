# 04 - Branch Protection Rules and Merge Queues

## 1. Production Branch Protection Standards

Configuring protection on `main` ensures no broken code reaches production:

- **Require a pull request before merging:** Direct pushes to `main` are strictly blocked.
- **Require approvals:** Minimum of 1 to 2 peer approvals.
- **Dismiss stale approvals when new commits are pushed:** Forces re-approval if someone changes code after approval.
- **Require status checks to pass:** Unit tests, integration tests, and linters must report green.
- **Require signed commits:** Rejects unauthenticated commits lacking GPG/SSH signatures.
- **Require linear history:** Enforces squash-merging or rebasing; prohibits messy merge commits.

---

## 2. GitHub Merge Queues

In busy engineering organizations where 50 PRs merge daily:
- PR A passes CI against `main`.
- PR B passes CI against `main`.
- When both merge, **their combined interaction breaks the build on `main`!**
- **Merge Queues** solve this by creating temporary integration branches where PRs are tested in serialized queue order before merging into `main`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Pull Requests and Merge Requests Review Excellence](./03-Pull-Requests-and-Merge-Requests-Review-Excellence.md) | [Index](../../../README.md) | [05 - CODEOWNERS Architecture and Governance →](./05-CODEOWNERS-Architecture-and-Governance.md) |
