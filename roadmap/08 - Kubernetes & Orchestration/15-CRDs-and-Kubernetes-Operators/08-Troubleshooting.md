# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Operator Issues

```bash
# 1. Check if CRD is registered and schema is valid
kubectl get crd <crd-name> -o yaml

# 2. Check operator controller logs
kubectl logs -n <operator-ns> -l control-plane=controller-manager -f

# 3. Force-remove stuck finalizer blocking object deletion
kubectl patch <kind> <name> -p '{"metadata":{"finalizers":null}}' --type=merge
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
