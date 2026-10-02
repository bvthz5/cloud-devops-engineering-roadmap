# 01 - Ansible Variable Precedence (22 Levels)

When the same variable is defined in multiple places, Ansible resolves it according to a strict 22-level hierarchy. **Highest level wins!**

```text
       VARIABLE PRECEDENCE (Lowest -> Highest)
 1. role defaults (defined in role/defaults/main.yml)  [LOWEST]
 ...
 6. group_vars/all
 7. group_vars/webservers
 ...
 12. host_vars/web1
 13. host facts / inventory vars
 ...
 16. play vars
 ...
 20. role vars (defined in role/vars/main.yml)
 21. task vars
 22. extra vars (-e / --extra-vars CLI flag)            [HIGHEST]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Ansible Facts](./02-Ansible-Facts-and-Gathering-System-Metadata.md) |
