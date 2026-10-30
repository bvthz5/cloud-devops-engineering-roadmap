# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Helm Debugging

```bash
# 1. Test template rendering locally without touching cluster
helm template my-release ./my-chart --debug

# 2. Perform dry-run against the live Kubernetes API
helm install my-release ./my-chart --dry-run=server

# 3. View user-supplied values of an active release
helm get values my-release

# 4. View all deployed manifests of an active release
helm get manifest my-release
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
