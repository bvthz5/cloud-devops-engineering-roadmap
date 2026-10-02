# 12 - Quick-Revision & Enterprise Cheat Sheet

## Module Quick Reference

| Concept | Description |
|---|---|
| Root Module | Directory where `terraform apply` runs |
| Child Module | Reusable component called via `module {}` block |
| Module Source | Local path, registry, Git, S3, or GCS |
| Module Composition | Chaining module outputs as inputs to other modules |

## Module Sources

| Source Type | Example |
|---|---|
| Local | `source = "./modules/vpc"` |
| Registry | `source = "terraform-aws-modules/vpc/aws"` |
| GitHub | `source = "github.com/org/repo?ref=v1.0"` |
| Git SSH | `source = "git@github.com:org/repo.git"` |
| S3 | `source = "s3::https://bucket.s3.amazonaws.com/module.zip"` |

## Design Pattern Decision

```text
Single resource? → Thin Wrapper Module
Complete solution? → Opinionated Module
Maximum flexibility? → Composable Module Pattern
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (06-Provisioners-and-Local-Exec) →](../06-Provisioners-and-Local-Exec/01-Provisioner-Types-local-exec-remote-exec-file.md) |
