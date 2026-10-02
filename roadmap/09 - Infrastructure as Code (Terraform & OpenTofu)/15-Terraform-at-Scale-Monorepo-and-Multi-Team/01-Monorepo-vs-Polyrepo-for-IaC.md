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
| [README](./README.md) | [README](./README.md) | [02 - Multi-Team Patterns](./02-Multi-Team-Collaboration-Patterns.md) |
