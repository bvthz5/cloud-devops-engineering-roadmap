# 06 - Structuring Production Ansible Repositories

Best practice production repository structure:
```text
ansible-repo/
├── ansible.cfg
├── site.yml
├── production.yml
├── staging.yml
├── inventory/
│   ├── production/
│   └── staging/
├── roles/
└── collections/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Reusing Content include_role import_role include_tasks import_tasks](./05-Reusing-Content-include_role-import_role-include_tasks-import_tasks.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
