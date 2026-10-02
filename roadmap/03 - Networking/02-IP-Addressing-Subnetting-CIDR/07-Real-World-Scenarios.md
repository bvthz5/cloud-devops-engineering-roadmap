# 07 — Real-World Subnetting Production Scenarios

---

## Scenario 1: Kubernetes EKS Pod IP Exhaustion Outage

### Incident Summary
A Kubernetes EKS cluster uses AWS VPC CNI. During a major deployment, newly created pods get stuck indefinitely in `ContainerCreating`.
`kubectl describe pod` shows:
`FailedCreatePodSandBox: failed to assign an IP address to container: no IP addresses available in subnet`.

### Root Cause Analysis
- The worker nodes were placed in `/24` subnets (251 usable IPs).
- In AWS VPC CNI, each Kubernetes Pod receives a real secondary private IP from the host node's VPC subnet.
- With 10 EC2 nodes running 30 pods each, all 251 available IPs in the subnet were consumed!

### Production Solution
1. **Short Term:** Add a secondary CIDR block (e.g. `100.64.0.0/16` CGNAT range) to the VPC and attach new subnets.
2. **Long Term:** Enable **EKS Prefix Delegation** (allocates a `/28` IP prefix of 16 IPs per ENI slot) or migrate to a CNI that supports overlay networking (e.g. Cilium / Calico).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - IPv6 Architecture and Migration](./06-IPv6-Architecture-and-Migration.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
