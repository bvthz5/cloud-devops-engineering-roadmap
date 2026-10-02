# 01 - Python DevOps Foundations and Environment

## 1. Modern Dependency Management: `venv` and `uv`

Never pollute the operating system's global Python environment with `sudo pip install`. Modern Linux (PEP 668) marks system environments as externally managed.

```bash
# Standard Python venv
python3 -m venv .venv
source .venv/bin/activate

# Blazing-fast modern alternative: uv (written in Rust, 10-100x faster than pip)
uv venv
uv pip install requests boto3 click
```

---

## 2. Type Hints in Automation

Type hints prevent runtime bugs before scripts execute in production:

```python
from typing import List, Dict, Optional

def get_unhealthy_hosts(threshold: float, cluster_nodes: List[Dict[str, float]]) -> List[str]:
    unhealthy: List[str] = []
    for node in cluster_nodes:
        if node.get("cpu_usage", 0.0) > threshold:
            unhealthy.append(str(node.get("hostname", "unknown")))
    return unhealthy
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Subprocess Execution](./02-Subprocess-Execution-and-System-Management.md) |
