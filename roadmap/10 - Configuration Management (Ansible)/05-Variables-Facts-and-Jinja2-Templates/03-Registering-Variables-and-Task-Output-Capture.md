# 03 - Registering Variables (`register`)

```yaml
- name: Run healthcheck command
  ansible.builtin.command: /usr/local/bin/check_status
  register: status_out
  ignore_errors: yes

- name: Print output if failed
  ansible.builtin.debug:
    msg: "Command failed with output: {{ status_out.stdout }}"
  when: status_out.rc != 0
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Ansible Facts](./02-Ansible-Facts-and-Gathering-System-Metadata.md) | [README](./README.md) | [04 - Jinja2 Templating](./04-Jinja2-Templating-Syntax-Filters-and-Control-Structures.md) |
