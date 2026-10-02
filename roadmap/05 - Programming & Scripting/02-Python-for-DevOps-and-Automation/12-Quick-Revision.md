# 12 - Python Automation: Quick Revision Cheat Sheet

## Safe Subprocess Pattern
```python
import subprocess

result = subprocess.run(
    ["binary", "arg1", "arg2"],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    text=True,
    check=True,
    timeout=30
)
```

## Resilient HTTP Session
```python
import requests
from requests.adapters import HTTPAdapter
from urllib3.util import Retry

s = requests.Session()
s.mount("https://", HTTPAdapter(max_retries=Retry(total=3, backoff_factor=1)))
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (03-Golang-Basics-for-Cloud-Native) →](../03-Golang-Basics-for-Cloud-Native/01-Why-Go-Dominates-Cloud-Native-Infrastructure.md) |
