# 01 - Playbook Structure, Plays & Tasks

```yaml
---
- name: Configure Web Servers
  hosts: webservers
  become: yes
  vars:
    http_port: 80

  tasks:
    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present

    - name: Ensure Nginx is running
      ansible.builtin.service:
        name: nginx
        state: started
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-Ad-Hoc-Commands-and-Core-Modules)](../03-Ad-Hoc-Commands-and-Core-Modules/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Idempotency Principles and Changed Status →](./02-Idempotency-Principles-and-Changed-Status.md) |
