# 07 - Real-World Scenarios

## Scenario 01: Wrong Workspace, Wrong Apply

### Incident
An engineer forgot to switch workspaces and ran `terraform apply` intended for dev against the production workspace. Production database parameters were changed to dev-level settings, causing performance degradation.

### Resolution
1. Immediately re-applied with production tfvars
2. Implemented workspace-based CI/CD pipelines (no manual workspace switching)
3. Added Sentinel policy: `terraform.workspace must match branch name`
4. Migrated to directory-based environments for stronger isolation

---

## Scenario 02: Directory-Based Migration

### Challenge
A growing team needed different infrastructure configurations per environment but was using workspaces with conditionals everywhere, making code unreadable.

### Solution
Migrated to directory-based structure with shared modules. Each environment has its own backend, tfvars, and can diverge in structure when needed.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Multi Account Multi Region Deployment](./06-Multi-Account-Multi-Region-Deployment.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
