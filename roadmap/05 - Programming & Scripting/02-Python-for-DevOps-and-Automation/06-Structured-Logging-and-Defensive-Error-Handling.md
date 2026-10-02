# 06 - Structured Logging and Defensive Error Handling

## 1. JSON Structured Logging for Cloud Observability

Printing raw text (`print("Server started")`) makes log parsing in Datadog, CloudWatch, or Grafana Loki painful. Output structured JSON logs:

```python
import json
import logging
import sys
from datetime import datetime

class JsonFormatter(logging.Formatter):
    def format(self, record):
        log_record = {
            "timestamp": datetime.utcnow().isoformat() + "Z",
            "level": record.levelname,
            "message": record.getMessage(),
            "logger": record.name,
            "line": record.lineno
        }
        if hasattr(record, "extra_data"):
            log_record["extra"] = record.extra_data
        return json.dumps(log_record)

logger = logging.getLogger("automation")
handler = logging.StreamHandler(sys.stdout)
handler.setFormatter(JsonFormatter())
logger.addHandler(handler)
logger.setLevel(logging.INFO)

# Log event with extra context
logger.info("Successfully refreshed IAM tokens", extra={"extra_data": {"account_id": "123456789"}})
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Config Parsing](./05-Config-Parsing-JSON-YAML-and-TOML.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
