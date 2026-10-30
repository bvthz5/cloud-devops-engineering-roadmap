# 09 - Interview Questions & Scenarios

### Q1: Why does HashiCorp recommend avoiding provisioners?
**Answer:**
Provisioners break the declarative model, require network access (SSH/WinRM), introduce non-idempotent side effects, and create hidden dependencies. Cloud-native alternatives (user_data, cloud-init, Packer) are more reliable, secure (no SSH needed), and idempotent.

---

### Q2: What is the difference between null_resource and terraform_data?
**Answer:**
Both serve as containers for provisioners with trigger-based re-execution. `terraform_data` (>= 1.4) is the modern built-in replacement that doesn't require the hashicorp/null provider. It uses `triggers_replace` instead of `triggers` and stores data via `input`/`output` attributes.

---

### Q3: When is local-exec acceptable in production?
**Answer:**
Legitimate uses include: running database migrations (Flyway/Liquibase), generating Ansible inventory files, calling external APIs for registration/deregistration, and generating local artifacts (kubeconfig, SSH config). The key criteria: the action runs on the operator's machine, not the target resource.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
