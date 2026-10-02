# 04 - SSL/TLS Certificate Expiration Scanner Template

```python
#!/usr/bin/env python3
# ssl_expiry_scanner.py
import ssl
import socket
from datetime import datetime

DOMAINS = ["google.com", "github.com", "example.com"]
ALERT_THRESHOLD_DAYS = 30

def check_ssl_expiry(hostname: str, port: int = 443):
    context = ssl.create_default_context()
    with socket.create_connection((hostname, port), timeout=5) as sock:
        with context.wrap_socket(sock, server_hostname=hostname) as ssock:
            cert = ssock.getpeercert()
            expiry_str = cert["notAfter"]
            expiry_date = datetime.strptime(expiry_str, "%b %d %H:%M:%S %Y %Z")
            days_left = (expiry_date - datetime.utcnow()).days
            
            if days_left <= ALERT_THRESHOLD_DAYS:
                print(f"[CRITICAL] Certificate for {hostname} expires in {days_left} days! ({expiry_date})")
            else:
                print(f"[OK] Certificate for {hostname} is valid for {days_left} more days.")

if __name__ == "__main__":
    for d in DOMAINS:
        try:
            check_ssl_expiry(d)
        except Exception as e:
            print(f"[ERROR] Failed to check {d}: {e}")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - K8s Auto-Remediator](./03-Kubernetes-Pod-Health-Auto-Remediator-Template.md) | [README](./README.md) | [05 - Slack Alert Dispatcher](./05-Slack-and-PagerDuty-Alert-Dispatcher-Template.md) |
