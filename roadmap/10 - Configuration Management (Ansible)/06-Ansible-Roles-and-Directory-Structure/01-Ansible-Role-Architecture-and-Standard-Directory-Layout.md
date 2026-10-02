# 01 - Ansible Role Architecture & Standard Directory Layout

## Role Directory Layout
```text
roles/common/
├── defaults/
│   └── main.yml      # Lowest priority default variables
├── vars/
│   └── main.yml      # High priority role variables
├── tasks/
│   └── main.yml      # Entrypoint for task execution
├── handlers/
│   └── main.yml      # Handlers defined for this role
├── templates/
│   └── app.conf.j2   # Jinja2 templates used by template module
├── files/
│   └── license.txt   # Static files copied by copy module
├── meta/
│   └── main.yml      # Role metadata and dependencies
└── tests/
    ├── inventory
    └── test.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Variables-Facts-and-Jinja2-Templates)](../05-Variables-Facts-and-Jinja2-Templates/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Creating Roles with ansible galaxy role init →](./02-Creating-Roles-with-ansible-galaxy-role-init.md) |
