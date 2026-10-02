# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Stuck Finalizer Deadlock

### Incident Summary
A cluster administrator deleted a test namespace containing an experimental custom operator. The namespace remained stuck in the `Terminating` status for 5 days, blocking CI/CD pipelines from recreating it.

### Root Cause
1. A custom resource had a finalizer: `my-org.com/teardown`.
2. The administrator deleted the operator Deployment *before* deleting the custom resources.
3. Because the operator was no longer running, no process existed to perform the cleanup and remove the finalizer!

### Remediation
Manually stripped the finalizer via JSON patch:
```bash
kubectl patch database test-db -p '{"metadata":{"finalizers":null}}' --type=merge
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - OLM & OperatorHub](./06-Operator-Lifecycle-Manager-OLM-and-OperatorHub.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
