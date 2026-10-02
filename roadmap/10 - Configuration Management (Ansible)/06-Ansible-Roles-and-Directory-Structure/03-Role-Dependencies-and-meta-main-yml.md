# 03 - Role Dependencies & `meta/main.yml`

```yaml
# roles/webserver/meta/main.yml
dependencies:
  - role: common
    vars:
      common_setting: true
  - role: geerlingguy.nginx
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Creating Roles](./02-Creating-Roles-with-ansible-galaxy-role-init.md) | [README](./README.md) | [04 - defaults vs vars](./04-Role-Variable-Scoping-defaults-vs-vars.md) |
