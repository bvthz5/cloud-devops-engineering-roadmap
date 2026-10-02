# 11 - MCQ

### Question 1
What is the security advantage of OIDC federation over static AWS access keys?
- [ ] A) Faster execution
- [ ] B) No need for IAM roles
- [x] C) No long-lived credentials to leak; short-lived tokens auto-expire
- [ ] D) Eliminates the need for Terraform state

<details>
<summary>Explanation</summary>
OIDC federation allows CI systems to exchange short-lived identity tokens for temporary AWS credentials. No static access keys exist to be leaked, stolen, or rotated.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
