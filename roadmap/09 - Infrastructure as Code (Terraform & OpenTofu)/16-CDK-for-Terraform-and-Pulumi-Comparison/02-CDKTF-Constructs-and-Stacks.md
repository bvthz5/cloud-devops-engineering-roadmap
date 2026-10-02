# 02 - CDKTF Constructs & Stacks

## Constructs vs HCL Modules

| CDKTF Concept | HCL Equivalent |
|---|---|
| Construct | Module |
| Stack | Root module |
| App | Multi-root workspace |
| `cdktf synth` | Generates `cdktf.out/` with Terraform JSON |
| `cdktf deploy` | `terraform apply` |
| `cdktf diff` | `terraform plan` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - CDKTF Architecture](./01-CDKTF-Architecture-and-Setup.md) | [README](./README.md) | [03 - Pulumi Architecture](./03-Pulumi-Architecture-and-State.md) |
