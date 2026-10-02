# 03 - Inventory Variables: group_vars/ and host_vars/

## Directory Layout Pattern
```text
project/
├── inventory/
│   ├── hosts.yml
│   ├── group_vars/
│   │   ├── all.yml
│   │   ├── webservers.yml
│   │   └── dbservers.yml
│   └── host_vars/
│       ├── web1.example.com.yml
│       └── db1.example.com.yml
└── site.yml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Host Groups](./02-Host-Groups-Nested-Groups-and-Group-Vars.md) | [README](./README.md) | [04 - Dynamic Inventory Plugins](./04-Dynamic-Inventory-Plugins-vs-Legacy-Scripts.md) |
