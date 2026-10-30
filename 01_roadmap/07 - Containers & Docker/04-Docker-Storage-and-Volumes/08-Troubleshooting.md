# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Named Volume Data on Linux

```bash
# Locate volume path on host
docker volume inspect pg_data --format '{{.Mountpoint}}'
# Output: /var/lib/docker/volumes/pg_data/_data

# Verify permissions and files
sudo ls -la /var/lib/docker/volumes/pg_data/_data
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
