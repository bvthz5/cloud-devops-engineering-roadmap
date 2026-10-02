# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Debugging Docker Hub Rate Limits (`HTTP 429`)

Anonymous users on Docker Hub are limited to 100 pulls per 6 hours per IP address. In shared CI/CD runners (like GitHub Actions), this limit is frequently exhausted:

```bash
# Check current remaining rate limit headers without pulling
curl -s -D - "https://auth.docker.io/token?service=registry.docker.io&scope=repository:ratelimitpreview/test:pull" | \
  jq -r .token | \
  xargs -I {} curl -I -H "Authorization: Bearer {}" https://registry-1.docker.io/v2/ratelimitpreview/test/manifests/latest 2>&1 | \
  grep -i "ratelimit"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
