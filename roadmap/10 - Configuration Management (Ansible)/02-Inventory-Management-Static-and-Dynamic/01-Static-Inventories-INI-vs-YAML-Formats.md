# 01 - Static Inventories: INI vs YAML Formats

## 1. INI Format Syntax
```ini
[webservers]
web1.example.com ansible_host=192.168.1.10 ansible_user=ubuntu
web2.example.com ansible_host=192.168.1.11

[dbservers]
db1.example.com ansible_host=192.168.1.20

[datacenter:children]
webservers
dbservers
```

---

## 2. YAML Format Syntax
```yaml
all:
  children:
    webservers:
      hosts:
        web1.example.com:
          ansible_host: 192.168.1.10
          ansible_user: ubuntu
        web2.example.com:
          ansible_host: 192.168.1.11
    dbservers:
      hosts:
        db1.example.com:
          ansible_host: 192.168.1.20
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-Ansible-Architecture-Agentless-Push)](../01-Ansible-Architecture-Agentless-Push/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Host Groups Nested Groups and Group Vars →](./02-Host-Groups-Nested-Groups-and-Group-Vars.md) |
