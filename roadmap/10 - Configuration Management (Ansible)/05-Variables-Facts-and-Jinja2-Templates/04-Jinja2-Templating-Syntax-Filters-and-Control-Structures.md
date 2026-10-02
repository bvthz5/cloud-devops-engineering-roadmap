# 04 - Jinja2 Templating Syntax & Control Structures

## `templates/nginx.conf.j2`
```nginx
server {
    listen {{ http_port | default(80) }};
    server_name {{ ansible_facts['fqdn'] }};

    location / {
        proxy_pass http://127.0.0.1:{{ app_port }};
    }
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Registering Variables and Task Output Capture](./03-Registering-Variables-and-Task-Output-Capture.md) | [Index](../../../README.md) | [05 - Built in Jinja2 Filters and Custom Filters →](./05-Built-in-Jinja2-Filters-and-Custom-Filters.md) |
