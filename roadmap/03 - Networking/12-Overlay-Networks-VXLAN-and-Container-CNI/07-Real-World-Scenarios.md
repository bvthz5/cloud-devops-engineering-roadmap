# 07 - Overlay Networks & CNI: Real-World Production Scenarios

## Scenario 1: AWS VPC CNI IP Address Exhaustion in EKS

### Incident Summary
During an auto-scaling traffic burst, new Kubernetes Pods remained stuck in `ContainerCreating` with events:
```
Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox: no IP addresses available
```

### Root Cause
AWS VPC CNI assigns real private VPC IPs to every Pod. The VPC subnet was allocated as a `/22` (1,022 usable IPs). As replica sets scaled up across 30 nodes, all 1,022 IP addresses were consumed, preventing any new Pod or EC2 instance from launching.

### Resolution
Enabled **Custom Networking in AWS VPC CNI**: Configured Pods to receive IPs from a secondary, non-routable CIDR block (`100.64.0.0/16`) while node EC2 instances retained primary VPC IPs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Network Policies](./06-Network-Policies-and-Micro-Segmentation.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
