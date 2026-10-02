# 02 - Static Analysis with `ansible-lint` & `yamllint`

```bash
# Run ansible-lint
ansible-lint site.yml
```

Configuration in `.ansible-lint`:
```yaml
skip_list:
  - name-prefix[reason]
enable_list:
  - no-same-owner
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Testing Strategy](./01-Ansible-Testing-Strategy-Linting-Unit-Integration-System.md) | [README](./README.md) | [03 - Molecule Framework Architecture](./03-Molecule-Framework-Architecture-and-Drivers.md) |
