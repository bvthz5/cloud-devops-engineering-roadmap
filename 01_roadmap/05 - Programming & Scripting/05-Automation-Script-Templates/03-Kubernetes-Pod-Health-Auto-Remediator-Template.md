# 03 - Kubernetes Pod Health Auto-Remediator Template

```python
#!/usr/bin/env python3
# k8s_pod_remediator.py
import subprocess
import json

def get_crashloop_pods(namespace="production"):
    cmd = ["kubectl", "get", "pods", "-n", namespace, "-o", "json"]
    res = subprocess.run(cmd, capture_output=True, text=True, check=True)
    data = json.loads(res.stdout)

    for item in data.get("items", []):
        pod_name = item["metadata"]["name"]
        statuses = item.get("status", {}).get("containerStatuses", [])
        for c in statuses:
            waiting = c.get("state", {}).get("waiting", {})
            if waiting.get("reason") in ["CrashLoopBackOff", "ImagePullBackOff"]:
                restart_count = c.get("restartCount", 0)
                print(f"[REMEDIATION] Pod {pod_name} is stuck in {waiting.get('reason')} (Restarts: {restart_count})")
                remediate_pod(namespace, pod_name)

def remediate_pod(namespace, pod_name):
    print(f"Collecting logs before pod termination...")
    subprocess.run(["kubectl", "logs", pod_name, "-n", namespace, "--tail=50"])
    print(f"Deleting broken pod {pod_name} to trigger replica restart...")
    subprocess.run(["kubectl", "delete", "pod", pod_name, "-n", namespace, "--grace-period=0", "--force"])

if __name__ == "__main__":
    get_crashloop_pods()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Cloud Snapshot and Backup Retention Template](./02-Cloud-Snapshot-and-Backup-Retention-Template.md) | [Index](../../../README.md) | [04 - SSL TLS Certificate Expiration Scanner Template →](./04-SSL-TLS-Certificate-Expiration-Scanner-Template.md) |
