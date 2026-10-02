# 05 - Config Parsing: JSON, YAML, and TOML

## 1. Parsing and Writing YAML Safely

Standard `yaml.load()` is vulnerable to arbitrary code execution. Always use `yaml.safe_load()`:

```python
import yaml

# Read Kubernetes manifest
with open("deployment.yaml", "r", encoding="utf-8") as f:
    manifest = yaml.safe_load(f)

# Update replica count programmatically
manifest["spec"]["replicas"] = 5

# Save back to disk
with open("deployment.yaml", "w", encoding="utf-8") as f:
    yaml.dump(manifest, f, default_flow_style=False, sort_keys=False)
```

---

## 2. Modern Schema Validation with Pydantic

```python
from pydantic import BaseModel, Field

class DatabaseConfig(BaseModel):
    host: str
    port: int = Field(default=5432, ge=1024, le=65535)
    username: str
    ssl_enabled: bool = True

# Automatically validates types and constraints
config = DatabaseConfig(host="db.internal", port=5432, username="postgres")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - REST API Automation](./04-REST-API-Automation-requests-and-httpx.md) | [README](./README.md) | [06 - Structured Logging](./06-Structured-Logging-and-Defensive-Error-Handling.md) |
