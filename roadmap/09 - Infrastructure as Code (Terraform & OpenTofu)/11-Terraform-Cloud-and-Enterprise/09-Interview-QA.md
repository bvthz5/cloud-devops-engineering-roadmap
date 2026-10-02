# 09 - Interview Questions

### Q1: What are the benefits of Terraform Cloud over CLI-only Terraform?
**Answer:**
Centralized state management with encryption, VCS-driven speculative plans on PRs, Sentinel policy enforcement, cost estimation, team RBAC, audit logging, and cross-workspace run triggers. It eliminates the need for engineers to manage backends, state locking, and CI/CD integration manually.

---

### Q2: When would you choose Terraform Enterprise over Terraform Cloud?
**Answer:**
When compliance requirements mandate that all infrastructure operations execute within your private network (air-gapped environments), when you need custom SAML/SSO integration, or when regulatory frameworks prohibit SaaS-based state management.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
