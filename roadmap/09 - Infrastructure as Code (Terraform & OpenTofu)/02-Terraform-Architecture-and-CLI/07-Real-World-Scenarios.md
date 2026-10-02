# 07 - Real-World Scenarios & Post-Mortems

## Scenario 01: Provider Version Mismatch Across Teams

### Incident
Team A pinned AWS provider `~> 4.0` while Team B used `~> 5.0`. Both teams shared a common module that broke silently when provider v5 renamed several resource attributes.

### Resolution
1. Adopted organization-wide provider version policy
2. Enforced `.terraform.lock.hcl` committed to all repos
3. Created CI checks that fail if lock file is missing

---

## Scenario 02: Backend Migration Data Loss Scare

### Incident
During migration from local backend to S3, an engineer ran `terraform init` without `-migrate-state`. The new empty S3 state was initialized, and `terraform plan` showed ALL resources as "to be created" (1,200 resources).

### Resolution
1. Stopped before `apply` — no actual damage
2. Recovered local state file from `.terraform.tfstate.backup`
3. Re-ran `terraform init -migrate-state` correctly
4. Added CI pipeline that blocks `terraform init` without proper flags

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Version Management](./06-Version-Management-tfenv-tofuenv-and-Required-Versions.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
