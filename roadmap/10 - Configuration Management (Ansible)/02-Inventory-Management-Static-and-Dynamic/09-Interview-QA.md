# 09 - Interview Q&A: Inventory Management

### Q: How does Ansible determine variable precedence between `group_vars/all` and `group_vars/webservers`?
**Answer:** Child groups override parent groups, so `group_vars/webservers` overrides `group_vars/all`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
