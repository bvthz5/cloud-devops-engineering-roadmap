# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Merged Configuration

When debugging complex multi-file overrides or variable interpolations, use `docker compose config` to output the exact final merged YAML parsed by Docker:

```bash
docker compose -f compose.yaml -f compose.prod.yaml config
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
