# 05 - Reusing Content: `import_*` vs `include_*`

- **Static Import (`import_tasks`, `import_role`)**: Evaluated at **playbook parse time**. Loops and variable-based task names are NOT supported.
- **Dynamic Include (`include_tasks`, `include_role`)**: Evaluated at **playbook execution runtime**. Fully supports loops and dynamic variables.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - defaults vs vars](./04-Role-Variable-Scoping-defaults-vs-vars.md) | [README](./README.md) | [06 - Production Repositories](./06-Structuring-Production-Ansible-Repositories.md) |
