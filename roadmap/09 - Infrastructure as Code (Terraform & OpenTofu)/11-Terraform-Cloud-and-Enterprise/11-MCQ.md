# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What is a "speculative plan" in Terraform Cloud?
- [ ] A) A plan that estimates costs
- [x] B) A read-only plan triggered by a PR that cannot be applied
- [ ] C) A plan that skips validation
- [ ] D) A plan that runs on self-hosted agents

<details>
<summary>Explanation</summary>
A speculative plan is a read-only terraform plan triggered when a PR is opened. It shows the plan output in the PR but cannot be applied directly. It's used for review purposes.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
