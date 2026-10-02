# 04 - State Management & Remote Backends

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Terraform-State-Purpose-Structure-and-Internals.md` — What the state file contains, JSON structure, resource metadata, serial numbers, and lineage.
2. `02-Remote-State-Backends-S3-GCS-AzureRM-Consul.md` — Configuring remote backends, state locking, encryption at rest, and cross-team state sharing.
3. `03-State-Locking-Concurrency-and-Force-Unlock.md` — How state locking prevents corruption, DynamoDB for AWS, built-in locking for GCS/Azure, and force-unlock procedures.
4. `04-State-Commands-mv-rm-import-taint-untaint.md` — State manipulation: moving resources, removing from state, importing existing infrastructure, and taint/untaint (deprecated replaced).
5. `05-Sensitive-Data-in-State-and-Encryption.md` — State file security: sensitive outputs, encryption at rest, encryption in transit, and OpenTofu client-side encryption.
6. `06-State-Splitting-and-Multi-State-Architecture.md` — Breaking monolithic state into domain-specific states, terraform_remote_state data source, and cross-state dependencies.
7. `07-Real-World-Scenarios.md` — State corruption recovery, accidental state deletion, and cross-account state sharing.
8. `08-Troubleshooting.md` — State-related errors and fix procedures.
9. `09-Interview-QA.md` — Interview questions on state management.
10. `10-Hands-On-Practice.md` — Labs: Backend migration, state import, and state manipulation.
11. `11-MCQ.md` — Scenario-based MCQs on state.
12. `12-Quick-Revision.md` — State management cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 03 - HCL Syntax](../03-HCL-Syntax-Variables-and-Outputs/README.md) | [README](./README.md) | [01 - State Purpose & Structure](./01-Terraform-State-Purpose-Structure-and-Internals.md) |
