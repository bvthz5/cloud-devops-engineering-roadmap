# 09 - Interview Questions

### Q1: How do you scale Terraform for a 100-team organization?
**Answer:** Split state by domain/team, use a producer-consumer module model with a private registry, enforce policies with Sentinel/OPA, use CODEOWNERS for module ownership, implement self-service via an internal platform, and centralize CI/CD with Terraform Cloud or Atlantis.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
