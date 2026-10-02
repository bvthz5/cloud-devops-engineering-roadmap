# 02 - Ansible Facts & Gathering System Metadata

Ansible automatically collects managed node hardware, network, and OS facts at the start of a play via the `setup` module.

```yaml
- name: Inspect Facts
  hosts: all
  gather_facts: yes
  tasks:
    - name: Print OS Architecture
      ansible.builtin.debug:
        msg: "Host {{ ansible_facts['hostname'] }} runs {{ ansible_facts['os_family'] }}"
```

## Custom Facts (`/etc/ansible/facts.d/*.fact`)
Files returning JSON or INI placed in `/etc/ansible/facts.d/custom.fact` are populated under `ansible_facts.ansible_local.custom`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Variable Precedence](./01-Ansible-Variable-Precedence-22-Levels.md) | [README](./README.md) | [03 - Registering Variables](./03-Registering-Variables-and-Task-Output-Capture.md) |
