# 09 - Interview Questions

### Q1: How do you secure CI/CD pipelines that run Terraform?
**Answer:** Use OIDC federation for keyless authentication (no static credentials). Restrict IAM role trust policies to specific repos/branches. Use saved plans to ensure apply matches what was reviewed. Require manual approval gates for production applies.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
