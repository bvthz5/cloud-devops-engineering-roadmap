# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Misconfigured Admission Webhook Outage

### Incident Summary
A security team deployed a custom validating webhook to enforce image vulnerability scans. 30 minutes later, all cluster deployments failed to scale, and autoscaling crashed cluster-wide.

### Root Cause
1. The webhook manifest specified `failurePolicy: Fail`.
2. The webhook deployment had only 1 replica scheduled on worker node 4.
3. Worker node 4 ran out of disk space, terminating the webhook pod.
4. With the webhook offline, `kube-apiserver` could not get validation responses and rejected 100% of all incoming Pod creation requests.

### Remediation
1. Switched `failurePolicy` to `Ignore` during initial rollout.
2. Scaled the webhook deployment to 3 replicas with Pod Anti-Affinity and a PodDisruptionBudget.
3. Excluded the `kube-system` namespace from webhook evaluation using `namespaceSelector`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Policy as Code with Kyverno and OPA Gatekeeper](./06-Policy-as-Code-with-Kyverno-and-OPA-Gatekeeper.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
