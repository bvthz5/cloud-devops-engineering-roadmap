# 05 - Asynchronous Tasks (`async` & `poll`)

```yaml
- name: Long running database backup
  ansible.builtin.command: /usr/local/bin/backup_db.sh
  async: 3600
  poll: 0
  register: backup_job

- name: Check backup status later
  ansible.builtin.async_status:
    jid: "{{ backup_job.ansible_job_id }}"
  register: job_result
  until: job_result.finished
  retries: 60
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Execution Strategies linear free and host_pinned](./04-Execution-Strategies-linear-free-and-host_pinned.md) | [Index](../../../README.md) | [06 - Mitogen for Ansible 10x Speedup →](./06-Mitogen-for-Ansible-10x-Speedup.md) |
