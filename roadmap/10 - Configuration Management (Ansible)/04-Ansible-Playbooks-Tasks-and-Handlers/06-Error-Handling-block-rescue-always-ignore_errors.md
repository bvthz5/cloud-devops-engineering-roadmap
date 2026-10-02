# 06 - Error Handling: `block`, `rescue`, `always`

```yaml
- name: Database Migration Block
  block:
    - name: Run Schema Migration
      ansible.builtin.command: /usr/bin/db-migrate
  rescue:
    - name: Rollback Migration on Failure
      ansible.builtin.command: /usr/bin/db-rollback
  always:
    - name: Cleanup Lock File
      ansible.builtin.file:
        path: /tmp/db.lock
        state: absent
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Loops & Retries](./05-Loops-loop-with_items-until-and-retries.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
