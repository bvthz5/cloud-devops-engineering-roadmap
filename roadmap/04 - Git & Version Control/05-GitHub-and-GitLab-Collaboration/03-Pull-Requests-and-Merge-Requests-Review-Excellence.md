# 03 - Pull Requests and Merge Requests Review Excellence

## 1. High-Velocity Code Review Principles

A Pull Request (PR) is a request to review and integrate code into a protected branch.

### Best Practices:
1. **Small PR Sizes:** DORA research confirms PRs with $< 300$ lines of diff have an 80% faster time-to-merge and detect 2x more architectural defects.
2. **Draft PRs:** Open early Draft PRs to solicit architectural alignment before spending days on implementation.
3. **Automated CI First:** Automated linters, security scans, and unit tests must run **before** human reviewers are notified.

---

## 2. GitHub Suggestion Syntax in Reviews
Reviewers can suggest direct in-place code edits in comments:
````markdown
```suggestion
def calculate_tax(amount: float) -> float:
    return amount * 0.20
```
````
The PR author can click **"Commit suggestion"** directly from the GitHub UI with zero manual typing.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Forking vs Shared](./02-Forking-Workflow-vs-Shared-Branch-Model.md) | [README](./README.md) | [04 - Branch Protection](./04-Branch-Protection-Rules-and-Merge-Queues.md) |
