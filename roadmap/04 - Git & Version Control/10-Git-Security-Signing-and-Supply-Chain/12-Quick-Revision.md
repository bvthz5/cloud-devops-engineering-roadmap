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
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [11 - Git LFS](../11-Git-LFS-and-Artifact-Management/README.md) |
