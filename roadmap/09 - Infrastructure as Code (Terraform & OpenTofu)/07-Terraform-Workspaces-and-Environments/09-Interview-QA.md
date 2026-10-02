# 09 - Interview Questions & Scenarios

### Q1: Workspaces vs directories for managing environments - which do you prefer and why?
**Answer:**
For production environments (dev/staging/prod), I prefer directory-based separation because it provides physical isolation of state files, allows environments to diverge in configuration, and prevents accidental cross-environment operations. Workspaces are better suited for ephemeral or identical environments like feature-branch testing.

---

### Q2: How do you deploy the same infrastructure to multiple AWS accounts?
**Answer:**
Use provider aliases with `assume_role`. Define one provider per account, each assuming a deployment role in the target account. Pass the aliased provider to modules via the `providers` argument.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
