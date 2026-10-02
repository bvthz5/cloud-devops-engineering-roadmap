# 04 - Conditionals: `when` Clause

```yaml
- name: Install Apache on RHEL
  ansible.builtin.dnf:
    name: httpd
    state: present
  when: ansible_facts['os_family'] == 'RedHat'

- name: Install Apache on Debian
  ansible.builtin.apt:
    name: apache2
    state: present
  when: ansible_facts['os_family'] == 'Debian'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Handlers](./03-Handlers-and-Event-Driven-Notifications.md) | [README](./README.md) | [05 - Loops & Retries](./05-Loops-loop-with_items-until-and-retries.md) |
