# 04 - REST API Automation: requests and httpx

## 1. Resilient HTTP Sessions with Retries

Never issue raw `requests.get()` without timeouts or retries in automated scripts. Transient network blips will cause your pipeline to crash!

```python
import requests
from requests.adapters import HTTPAdapter
from urllib3.util import Retry

def create_resilient_session() -> requests.Session:
    session = requests.Session()
    retry_strategy = Retry(
        total=5,
        backoff_factor=1,  # Wait 1s, 2s, 4s, 8s, 16s
        status_forcelist=[429, 500, 502, 503, 504],
        allowed_methods=["HEAD", "GET", "OPTIONS"]
    )
    adapter = HTTPAdapter(max_retries=retry_strategy)
    session.mount("https://", adapter)
    session.mount("http://", adapter)
    return session

client = create_resilient_session()
response = client.get("https://api.github.com/orgs/my-org/repos", timeout=(3.05, 10))
data = response.json()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - CLI Tools](./03-Building-Production-CLI-Tools-argparse-and-click.md) | [README](./README.md) | [05 - Config Parsing](./05-Config-Parsing-JSON-YAML-and-TOML.md) |
