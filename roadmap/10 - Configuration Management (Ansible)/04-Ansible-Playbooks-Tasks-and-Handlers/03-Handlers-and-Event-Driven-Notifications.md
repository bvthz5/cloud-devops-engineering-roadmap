# 03 - Handlers & Event-Driven Notifications

Handlers run **at the end of a play** if and only if notified by a task that reported `changed`.

```yaml
tasks:
  - name: Update Nginx Config
    ansible.builtin.template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
    notify: Restart Nginx

handlers:
  - name: Restart Nginx
    ansible.builtin.service:
      name: nginx
      state: restarted
```

To run handlers immediately mid-play:
```yaml
- meta: flush_handlers
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Idempotency](./02-Idempotency-Principles-and-Changed-Status.md) | [README](./README.md) | [04 - Conditionals](./04-Conditionals-when-failed_when-changed_when.md) |
