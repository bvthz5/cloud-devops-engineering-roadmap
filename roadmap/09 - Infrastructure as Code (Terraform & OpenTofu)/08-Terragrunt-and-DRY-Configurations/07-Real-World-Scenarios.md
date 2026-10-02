# 07 - Real-World Scenarios

## Scenario 01: 50-Module Enterprise Deployment

### Challenge
An enterprise had 50 Terraform modules across 4 environments with massive backend/provider duplication and manual dependency ordering.

### Solution
Adopted Terragrunt with hierarchical config. Backend config reduced from 200 files to 1. Dependency ordering automated via `dependency` blocks. Full environment deployment in a single `run-all apply`.

---

## Scenario 02: Terragrunt Circular Dependency

### Incident
Module A depended on Module B, and Module B depended on Module A's outputs, creating a circular dependency that blocked `run-all plan`.

### Resolution
Refactored to extract shared data into a third module C that both A and B depend on, breaking the cycle.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - run-all & CI/CD](./06-Terragrunt-run-all-and-CI-CD-Integration.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
