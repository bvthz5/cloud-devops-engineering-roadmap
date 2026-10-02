# 12 - Quick Revision & Cheat Sheet

## Configuration Hierarchy
1. `ANSIBLE_CONFIG` env var
2. `./ansible.cfg`
3. `~/.ansible.cfg`
4. `/etc/ansible/ansible.cfg`

## Essential Commands

| Command | Action |
|---|---|
| `ansible all -m ping` | Ping all hosts in inventory |
| `ansible-config view` | Print current active ansible.cfg |
| `ansible-config dump` | Print all merged settings |
| `ansible-doc <module>` | Display documentation for a module |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (02-Inventory-Management-Static-and-Dynamic) →](../02-Inventory-Management-Static-and-Dynamic/01-Static-Inventories-INI-vs-YAML-Formats.md) |
