# 05 - Migration Between Tools

## Terraform to CDKTF

```bash
# Convert existing .tf files to CDKTF (TypeScript)
cdktf convert < main.tf
```

## Terraform to Pulumi

```bash
# Convert Terraform HCL to Pulumi
pulumi convert --from terraform --language python
```

## Pulumi to Terraform
No automated tool. Manual rewrite required.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - HCL vs General Purpose Language Tradeoffs](./04-HCL-vs-General-Purpose-Language-Tradeoffs.md) | [Index](../../../README.md) | [06 - Decision Framework Choosing the Right Tool →](./06-Decision-Framework-Choosing-the-Right-Tool.md) |
