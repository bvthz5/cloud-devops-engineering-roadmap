# 02 - Idempotency Principles & Changed Status

An idempotent task yields the exact same system state regardless of how many times it is executed.

```yaml
# Controlling changed status explicitly
- name: Seed database schema
  ansible.builtin.command: /usr/local/bin/seed_db.sh
  register: db_result
  changed_when: "'Database updated' in db_result.stdout"
  failed_when: db_result.rc != 0 and 'already exists' not in db_result.stderr
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Playbook Structure](./01-Playbook-Structure-Plays-Tasks-and-YAML-Syntax.md) | [README](./README.md) | [03 - Handlers & Notifications](./03-Handlers-and-Event-Driven-Notifications.md) |
