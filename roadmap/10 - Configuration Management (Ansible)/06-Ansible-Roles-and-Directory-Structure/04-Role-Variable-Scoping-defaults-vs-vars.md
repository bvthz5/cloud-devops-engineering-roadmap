# 04 - Role Variable Scoping: `defaults/` vs `vars/`

| Directory | Priority | Use Case | Overridable? |
|---|---|---|---|
| `defaults/main.yml` | **Lowest (Level 1)** | Default values intended for user customization | **YES** (by inventory, play vars, extra vars) |
| `vars/main.yml` | **High (Level 20)** | Constants internal to role operation | **NO** (Only by extra-vars) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Role Dependencies and meta main yml](./03-Role-Dependencies-and-meta-main-yml.md) | [Index](../../../README.md) | [05 - Reusing Content include_role import_role include_tasks import_tasks →](./05-Reusing-Content-include_role-import_role-include_tasks-import_tasks.md) |
