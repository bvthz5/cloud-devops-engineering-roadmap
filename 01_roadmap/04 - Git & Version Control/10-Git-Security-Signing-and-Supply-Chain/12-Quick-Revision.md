# 12 - Git Security: Quick Revision Cheat Sheet

## Security Configuration Matrix

| Task | Command |
|---|---|
| Configure SSH commit signing | `git config --global gpg.format ssh` |
| Set SSH signing key | `git config --global user.signingkey ~/.ssh/key.pub` |
| Auto-sign all commits | `git config --global commit.gpgsign true` |
| Verify commit signatures | `git log --show-signature` |
| Purge file from entire history | `git filter-repo --path <file> --invert-paths` |
| Scan history for secrets | `gitleaks detect --verbose` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (11-Git-LFS-and-Artifact-Management) →](../11-Git-LFS-and-Artifact-Management/01-The-Large-Binary-File-Problem-in-Git.md) |
