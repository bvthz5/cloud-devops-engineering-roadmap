# 01 - Ansible Testing Strategy (Pyramid)

```text
                  TESTING PYRAMID
                 ─────────────────
                    /   E2E   \        (AWX Job Runs)
                   /----------- \
                  / Integration  \     (Molecule + Docker)
                 /----------------\
                / Static Analysis  \   (ansible-lint, yamllint)
               /--------------------\
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Static Analysis & ansible-lint](./02-Static-Analysis-with-ansible-lint-and-yamllint.md) |
