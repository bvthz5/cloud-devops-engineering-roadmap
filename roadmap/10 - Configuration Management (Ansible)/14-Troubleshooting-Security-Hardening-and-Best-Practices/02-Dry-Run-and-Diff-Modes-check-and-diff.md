# 02 - Dry-Run (`--check`) & Diff (`--diff`) Modes

```bash
ansible-playbook site.yml --check --diff
```

To force a task to run even during `--check`:
```yaml
- name: Read status file
  ansible.builtin.command: cat /etc/app_status
  check_mode: no
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Ansible Debugging Techniques and Verbosity Levels](./01-Ansible-Debugging-Techniques-and-Verbosity-Levels.md) | [Index](../../../README.md) | [03 - Security Hardening and CIS Benchmark Automation →](./03-Security-Hardening-and-CIS-Benchmark-Automation.md) |
