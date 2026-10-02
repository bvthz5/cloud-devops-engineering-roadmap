# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Ingress Routing Failures

```text
[ Symptom: Client receives "404 Not Found" from Nginx ]
                         │
                         ▼
        Is the request reaching Ingress-Nginx?
        Check ingress controller logs:
        kubectl logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx
        ├── NO  ──► Check external LoadBalancer, DNS A records, or Cloud Firewall.
        └── YES ──► Inspect host and path matching:
                         │
                         ▼
        Does the 'Host' header match the Ingress rule?
        ├── Host header missing or wrong (e.g. hitting raw IP) ──► Hits "default backend" 404!
        └── Host matches, but pathType is wrong:
            Check if path requires regex rewrite or / trailing slash.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
