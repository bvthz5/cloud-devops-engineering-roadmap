# 10 - Hands-On Practice: Building an Automated Health Monitor

## Lab Scenario
Create a complete Python script that monitors API endpoints, executes retries, and outputs structured JSON logs.

---

## Lab Steps

### Step 1: Write `health_monitor.py`
```python
cat << 'EOF' > /tmp/health_monitor.py
import json
import sys
import requests
from requests.adapters import HTTPAdapter
from urllib3.util import Retry

def check_endpoints(urls: list[str]) -> bool:
    session = requests.Session()
    retries = Retry(total=3, backoff_factor=0.5, status_forcelist=[500, 502, 503, 504])
    session.mount("https://", HTTPAdapter(max_retries=retries))
    
    all_healthy = True
    for url in urls:
        try:
            res = session.get(url, timeout=5)
            log = {"url": url, "status": res.status_code, "latency_ms": round(res.elapsed.total_seconds() * 1000, 2)}
            print(json.dumps(log))
            if res.status_code >= 400:
                all_healthy = False
        except Exception as e:
            print(json.dumps({"url": url, "error": str(e), "healthy": False}), file=sys.stderr)
            all_healthy = False
    return all_healthy

if __name__ == "__main__":
    targets = ["https://httpbin.org/status/200", "https://httpbin.org/status/503"]
    success = check_endpoints(targets)
    sys.exit(0 if success else 1)
EOF
```

### Step 2: Execute and Validate Output
```bash
python3 /tmp/health_monitor.py
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
