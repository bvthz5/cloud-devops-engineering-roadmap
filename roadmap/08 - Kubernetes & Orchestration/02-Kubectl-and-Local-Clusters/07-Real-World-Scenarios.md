# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Dev/Prod Context Catastrophe

### Incident Summary
A senior engineer intending to purge experimental test pods in a staging cluster inadvertently deleted critical customer-facing deployments in the production cluster.

### Root Cause
The engineer had multiple terminal tabs open. In one tab, they had switched to `prod-cluster` context 2 hours prior to investigate an alert. Without checking the active context prompt, they ran `kubectl delete deployment --all`.

### Remediation & Architectural Guardrails
1. **Installed `kube-ps1` / Starship:** Configured terminal prompt to prominently display active cluster and namespace in bright red for production.
2. **Kube-score & Kube-no-trouble:** Enforced strict RBAC preventing developers from running wildcard deletions.
3. **Guardrails Plugin:** Deployed `kubectl-safe` wrapper requiring interactive confirmation before running write commands against contexts containing `prod`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Production Bootstrap with Kubeadm](./06-Production-Bootstrap-with-Kubeadm.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
