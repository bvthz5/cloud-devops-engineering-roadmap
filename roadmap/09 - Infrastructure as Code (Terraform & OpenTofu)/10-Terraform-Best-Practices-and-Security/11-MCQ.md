# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which tool generates least-privilege IAM policies by recording actual AWS API calls?
- [ ] A) tfsec
- [ ] B) Checkov
- [x] C) iamlive
- [ ] D) Sentinel

<details>
<summary>Explanation</summary>
iamlive acts as a proxy that records AWS API calls made during terraform apply and generates the minimal IAM policy needed. pike scans .tf files statically, while iamlive captures runtime usage.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
