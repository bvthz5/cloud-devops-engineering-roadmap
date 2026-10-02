# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Service Connectivity & DNS Failures

```text
[ Symptom: Application cannot reach "http://order-service" ]
                         │
                         ▼
        Can you resolve the DNS name?
        kubectl exec <client-pod> -- nslookup order-service
        ├── FAILS ──► Check CoreDNS pods: kubectl get pods -n kube-system -l k8s-app=kube-dns
        │             Check CoreDNS logs: kubectl logs -n kube-system -l k8s-app=kube-dns
        └── SUCCEEDS (Returns VIP 10.96.x.x)
                         │
                         ▼
        Does the Service have active endpoints?
        kubectl get endpoints <service-name>
        ├── <none> ──► Label selector mismatch! Compare service selector with pod labels.
        └── ACTIVE ──► Pod readiness probe is failing or container listening port is wrong.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
