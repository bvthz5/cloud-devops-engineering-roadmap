# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Velero Failures

```bash
# 1. View backup status and errors
velero backup describe <backup-name> --details

# 2. View granular backup logs
velero backup logs <backup-name> | grep -E "level=error"

# 3. Check storage location connectivity
velero backup-location get
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
