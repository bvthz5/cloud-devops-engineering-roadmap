# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: RBAC & Access Denied

```text
[ Symptom: "Error from server (Forbidden): pods is forbidden: User alice cannot list resource" ]
                                  │
                                  ▼
        Check effective permissions with kubectl auth can-i:
        kubectl auth can-i list pods -n <ns> --as=alice
                                  │
                                  ▼
        Inspect user bindings:
        kubectl get rolebindings,clusterrolebindings -n <ns> -o wide
        ├── Is the user or their group listed in 'subjects'?
        │   Check: openssl x509 -in alice.crt -text | grep -E "CN|O="
        └── Is the Role granting the exact verb ('get' vs 'list')?
            Remember: kubectl get requires BOTH 'get' and 'list'!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
