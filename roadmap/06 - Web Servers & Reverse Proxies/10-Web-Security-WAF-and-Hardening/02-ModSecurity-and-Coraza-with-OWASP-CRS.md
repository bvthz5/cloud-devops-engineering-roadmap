# 02 - ModSecurity and Coraza with OWASP CRS

## 1. The OWASP Core Rule Set (CRS)

The **OWASP Core Rule Set (CRS)** is the global industry standard for generic attack detection. It detects zero-day vulnerabilities across the entire OWASP Top 10 with minimal false positives.

### Anomaly Scoring Mode
Rather than blocking immediately on the first rule match (Traditional Mode), modern OWASP CRS uses **Anomaly Scoring**:
- Each matched rule adds an anomaly score (Critical = 5, Error = 4, Warning = 3, Notice = 2).
- At the end of Phase 2, if total score exceeds the inbound threshold (default: 5), the request is blocked with HTTP 403!

```text
Incoming Payload: "SELECT * FROM users WHERE id=1 OR 1=1"
  ├── Rule 942100 Matched (SQLi Attempt): Score += 5
  └── Total Anomaly Score = 5 >= Inbound Threshold (5) ➔ BLOCKED with 403!
```

---

## 2. Nginx with ModSecurity Integration

```nginx
# /etc/nginx/conf.d/waf.conf
server {
    listen 443 ssl;
    server_name secure.example.com;

    modsecurity on;
    modsecurity_rules_file /etc/nginx/modsec/main.conf;

    location / {
        proxy_pass http://backend_cluster;
    }
}
```

```apache
# /etc/nginx/modsec/main.conf
Include /etc/nginx/modsec/modsecurity.conf
Include /opt/owasp-crs/crs-setup.conf
Include /opt/owasp-crs/rules/*.conf

# Custom Exception Rule: Whitelist specific rule false positive on /api/upload
SecRule REQUEST_URI "@beginsWith /api/upload" \
    "id:1001,phase:1,pass,nolog,ctl:ruleRemoveById=942100"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - WAF Architecture](./01-Web-Application-Firewall-WAF-Architecture.md) | [README](./README.md) | [03 - HTTP Security Headers](./03-HTTP-Security-Headers-CSP-HSTS-Permissions-Policy.md) |
