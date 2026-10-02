# 05 - Large-Scale Migration Strategies

## 1. Migration Workflow

```text
Phase 1: Inventory
  - List all cloud resources (AWS Config, Cloud Asset Inventory)
  - Identify which are already managed by Terraform

Phase 2: Import
  - Write import blocks for unmanaged resources
  - Use -generate-config-out for bulk HCL generation
  - Review and refine generated code

Phase 3: Validate
  - terraform plan should show "No changes"
  - If changes appear, align HCL to match actual state

Phase 4: Organize
  - Split imported resources into logical modules
  - Use moved blocks to restructure without recreation
```

## 2. Tools for Large-Scale Import

| Tool | Description |
|---|---|
| **terraformer** | Import existing infrastructure into Terraform HCL |
| **aztfexport** | Azure-specific import tool |
| **gcloud terraform vet** | GCP resource import helper |
| **import blocks + generate-config** | Native Terraform approach |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Generate Config](./04-Generate-Config-from-Import.md) | [README](./README.md) | [06 - Refactoring Patterns](./06-Refactoring-Patterns-and-Best-Practices.md) |
