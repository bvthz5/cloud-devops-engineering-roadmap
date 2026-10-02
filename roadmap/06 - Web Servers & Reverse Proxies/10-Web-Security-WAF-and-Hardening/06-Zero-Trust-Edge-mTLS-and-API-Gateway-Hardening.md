# 06 - Zero-Trust Edge: mTLS and API Gateway Hardening

## 1. Path Traversal & Normalization Defense

Attackers attempt to access sensitive configuration files by injecting path traversal sequences into URLs:
`GET /static/../../etc/passwd`

Nginx automatically normalizes URIs before routing. However, when using `proxy_pass` with trailing slashes, misconfigurations can re-introduce traversal:

```nginx
# VULNERABLE CONFIGURATION:
location /files {
    alias /var/data/files/; # Notice missing trailing slash on location!
    # Request: GET /files../app.py ➔ Accesses /var/data/files/../app.py!
}

# HARDENED CONFIGURATION:
location /files/ {
    alias /var/data/files/;
}
```

---

## 2. Header Scrubbing at the Gateway

Ensure internal spoofing headers are stripped before passing traffic to backends:

```nginx
location / {
    proxy_pass http://backend;

    # Strip dangerous internal headers incoming from public clients
    proxy_set_header X-User-Role "";
    proxy_set_header X-Is-Admin "";
    proxy_set_header X-Internal-Token "";
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - DDoS Mitigation & Slowloris](./05-DDoS-Mitigation-Slowloris-and-Flood-Protection.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
