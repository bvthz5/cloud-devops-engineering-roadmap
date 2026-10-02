# 02 - Terraform & OpenTofu Architecture & CLI

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Terraform-Core-Architecture-Providers-and-Plugin-Protocol.md` — Core binary, provider plugin architecture, gRPC plugin protocol, and the dependency graph engine.
2. `02-Provider-Registry-Installation-and-Version-Pinning.md` — Terraform Registry, provider source addressing, version constraints, dependency lock file, and provider mirrors.
3. `03-Terraform-CLI-Deep-Dive-Init-Plan-Apply-Destroy.md` — Every `terraform` / `tofu` CLI subcommand with flags, environment variables, and operational best practices.
4. `04-Backend-Configuration-Local-S3-GCS-AzureRM.md` — Backend types, remote state storage (S3, GCS, Azure Blob), state locking with DynamoDB, and backend migration.
5. `05-Terraform-Graph-DAG-and-Parallelism.md` — Directed Acyclic Graph (DAG), resource dependency resolution, `terraform graph`, and `-parallelism` tuning.
6. `06-Version-Management-tfenv-tofuenv-and-Required-Versions.md` — Managing multiple Terraform/OpenTofu versions with tfenv/tofuenv, `.terraform-version`, and `required_version` constraints.
7. `07-Real-World-Scenarios.md` — Production incidents involving provider version mismatches, backend migrations, and parallelism bugs.
8. `08-Troubleshooting.md` — Diagnostic runbooks for init failures, provider errors, and backend issues.
9. `09-Interview-QA.md` — Senior DevOps interview scenarios on Terraform architecture.
10. `10-Hands-On-Practice.md` — Labs: Multi-provider setup, backend migration, and graph visualization.
11. `11-MCQ.md` — Scenario-based MCQs on architecture and CLI.
12. `12-Quick-Revision.md` — Architecture and CLI cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 01 - IaC Concepts](../01-IaC-Concepts-and-Evolution/README.md) | [README](./README.md) | [01 - Terraform Core Architecture](./01-Terraform-Core-Architecture-Providers-and-Plugin-Protocol.md) |
