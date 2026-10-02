# 01 - Monorepo vs Polyrepo for IaC

| Aspect | Monorepo | Polyrepo |
|---|---|---|
| **All IaC in one repo** | Yes | No (one repo per service/team) |
| **Cross-module changes** | Single PR | Multiple PRs across repos |
| **Code discovery** | Easy (everything in one place) | Harder (must search across repos) |
| **CI/CD complexity** | Path-based triggers needed | Simple per-repo pipelines |
| **Module sharing** | Local paths | Git refs or registry |
| **Access control** | CODEOWNERS file | Repository permissions |
| **Best for** | Small-medium orgs, platform teams | Large orgs with autonomous teams |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (14-CICD-for-Terraform-Atlantis-and-Pipelines)](../14-CICD-for-Terraform-Atlantis-and-Pipelines/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Multi Team Collaboration Patterns →](./02-Multi-Team-Collaboration-Patterns.md) |
