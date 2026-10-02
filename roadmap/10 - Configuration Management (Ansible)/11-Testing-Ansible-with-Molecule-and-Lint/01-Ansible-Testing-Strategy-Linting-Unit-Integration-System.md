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
| [← Prev Module (10-AWX-Ansible-Automation-Platform-and-Tower)](../10-AWX-Ansible-Automation-Platform-and-Tower/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Static Analysis with ansible lint and yamllint →](./02-Static-Analysis-with-ansible-lint-and-yamllint.md) |
