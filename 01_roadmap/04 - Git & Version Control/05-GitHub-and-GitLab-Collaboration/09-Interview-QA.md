# 09 - GitHub Collaboration: Interview Questions & Answers

### Q1: Why should an engineer use `git push --force-with-lease` instead of `git push --force`?
**Answer:** `git push --force` blindly overwrites the remote branch regardless of whether other developers pushed new commits to it. `git push --force-with-lease` checks if the remote reference matches your local tracking reference; if someone else pushed a commit in the interim, the force-push is rejected, preventing accidental code destruction.

### Q2: How does a GitHub Merge Queue prevent broken builds on `main`?
**Answer:** When multiple PRs pass CI individually against `main`, their merged union might still contain incompatible logic errors. A Merge Queue tests PRs sequentially on an integrated candidate branch before merging them into `main`, guaranteeing that `main` is never broken.

### Q3: What is the purpose of the `CODEOWNERS` file?
**Answer:** It defines which individuals or teams own specific paths, directories, or file patterns in a repository. GitHub automatically assigns these owners as reviewers and can enforce required approval before PRs modifying those files can be merged.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
