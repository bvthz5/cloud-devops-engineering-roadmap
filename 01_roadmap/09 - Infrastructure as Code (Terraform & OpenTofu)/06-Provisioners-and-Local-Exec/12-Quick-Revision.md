# 12 - Quick-Revision & Enterprise Cheat Sheet

## Provisioner Decision Matrix

| Need | Solution |
|---|---|
| Install packages on boot | cloud-init / user_data |
| Pre-bake machine images | Packer |
| Run local scripts after resource creation | local-exec provisioner |
| Run remote commands on instance | remote-exec (last resort) |
| Copy files to instance | file provisioner (last resort) |
| Trigger script on resource change | terraform_data + triggers_replace |
| Call external API for data | external data source |

## Provisioner Types

| Type | Runs On | Connection Required |
|---|---|---|
| `local-exec` | Operator machine | No |
| `remote-exec` | Target resource | SSH or WinRM |
| `file` | Target resource | SSH or WinRM |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (07-Terraform-Workspaces-and-Environments) →](../07-Terraform-Workspaces-and-Environments/01-Terraform-Workspaces-Architecture-and-CLI.md) |
