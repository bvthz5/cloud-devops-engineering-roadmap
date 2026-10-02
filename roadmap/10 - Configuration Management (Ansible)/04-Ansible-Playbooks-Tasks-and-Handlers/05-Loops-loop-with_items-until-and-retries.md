# 05 - Loops (`loop`) & Retries (`until`)

```yaml
# Standard Loop
- name: Install multiple packages
  ansible.builtin.apt:
    name: "{{ item }}"
    state: present
  loop:
    - curl
    - git
    - htop

# Retry until status 200
- name: Wait for API web service to be healthy
  ansible.builtin.uri:
    url: http://localhost:8080/health
    status_code: 200
  register: result
  until: result.status == 200
  retries: 10
  delay: 5
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Conditionals](./04-Conditionals-when-failed_when-changed_when.md) | [README](./README.md) | [06 - Error Handling](./06-Error-Handling-block-rescue-always-ignore_errors.md) |
