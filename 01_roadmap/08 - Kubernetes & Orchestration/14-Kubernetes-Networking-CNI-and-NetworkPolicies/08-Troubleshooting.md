# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: Cross-Node Pod Ping Failure

```text
[ Symptom: Pod A on Node 1 cannot ping Pod B on Node 2 ]
                         │
                         ▼
        Can Node 1 ping Node 2 on host network?
        ├── NO  ──► Cloud VPC security group or physical firewall is blocking traffic!
        └── YES ──► Check CNI encapsulation ports:
                         │
                         ▼
        Are CNI overlay ports open between worker nodes?
        ├── VXLAN: UDP port 8472 or 4789
        ├── Geneve: UDP port 6081
        ├── BGP: TCP port 179 (Calico)
        └── WireGuard: UDP port 51871
        Unblock these firewall ports to restore inter-pod communication!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
