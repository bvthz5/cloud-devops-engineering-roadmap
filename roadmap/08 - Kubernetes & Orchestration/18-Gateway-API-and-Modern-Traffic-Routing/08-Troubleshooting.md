# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Gateway API Status Conditions

```bash
# 1. Check Gateway programming status
kubectl get gateway prod-gateway -o yaml
# Look for:
# status.conditions:
#   - type: Programmed, status: "True"

# 2. Check HTTPRoute parent attachment
kubectl describe httproute my-route
# Look for:
# ParentRef: prod-gateway
# Conditions:
#   - type: Accepted, status: "True"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
