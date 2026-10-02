# 05 - Slack and PagerDuty Alert Dispatcher Template

```python
#!/usr/bin/env python3
# alert_dispatcher.py
import requests
import json
import os

SLACK_WEBHOOK_URL = os.environ.get("SLACK_WEBHOOK_URL", "")

def send_slack_alert(title: str, message: str, severity: str = "danger"):
    if not SLACK_WEBHOOK_URL:
        print("ERROR: SLACK_WEBHOOK_URL environment variable is missing.")
        return

    color_map = {
        "danger": "#FF0000",   # Red
        "warning": "#FFA500",  # Orange
        "good": "#00FF00"      # Green
    }

    payload = {
        "attachments": [{
            "color": color_map.get(severity, "#CCCCCC"),
            "title": f"🚨 {title}",
            "text": message,
            "fields": [
                {"title": "Environment", "value": "Production", "short": True},
                {"title": "Severity", "value": severity.upper(), "short": True}
            ],
            "footer": "DevOps SRE Automation Engine"
        }]
    }

    resp = requests.post(SLACK_WEBHOOK_URL, json=payload, timeout=5)
    resp.raise_for_status()
    print("Alert successfully dispatched to Slack.")

if __name__ == "__main__":
    send_slack_alert("High CPU Utilization on Node-04", "CPU usage has exceeded 92% for 10 minutes.", "danger")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - SSL Cert Scanner](./04-SSL-TLS-Certificate-Expiration-Scanner-Template.md) | [README](./README.md) | [06 - DB Latency Prober](./06-Database-Latency-and-Connection-Prober-Template.md) |
