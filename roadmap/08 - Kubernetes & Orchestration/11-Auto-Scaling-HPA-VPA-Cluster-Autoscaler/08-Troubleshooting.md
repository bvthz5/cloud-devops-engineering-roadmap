# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: HPA Showing `<unknown>`

```text
[ Symptom: kubectl get hpa shows "TARGETS: <unknown>/70%" ]
                         │
                         ▼
        Is metrics-server running in the cluster?
        kubectl get pods -n kube-system -l k8s-app=metrics-server
        ├── NO  ──► Install metrics-server via Helm or manifest!
        └── YES ──► Check metrics-server logs for TLS or probe issues:
                         │
                         ▼
        Do the pods in the target Deployment have resource requests defined?
        kubectl get deployment <name> -o yaml | grep -A 5 resources
        ├── NO  ──► Fix Deployment! HPA requires 'resources.requests.cpu'!
        └── YES ──► Check if pod metrics are available:
                    kubectl top pods
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
