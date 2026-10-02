# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Kubeconfig & Connectivity Failures

```text
[ Error: "error: You must be logged in to the server (Unauthorized)" ]
                         │
                         ▼
        Is the client certificate expired?
        openssl x509 -in ~/.kube/client.crt -text -noout | grep "Not After"
        ├── EXPIRED ──► Regenerate certificates or re-authenticate via OIDC/cloud CLI.
        └── VALID   ──► Check token expiration or revoked user credentials in API server.

[ Error: "The connection to the server <ip>:6443 was refused" ]
                         │
                         ▼
        Can you reach port 6443 on the remote endpoint?
        nc -zv <api-ip> 6443
        ├── NO  ──► Check cloud Security Group / Firewall rules.
        └── YES ──► Verify API server container is running on the master node.
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
