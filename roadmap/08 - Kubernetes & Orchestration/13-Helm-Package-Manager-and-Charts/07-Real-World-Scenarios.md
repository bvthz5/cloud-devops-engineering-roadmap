# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The `pending-upgrade` Deadlock

### Incident Summary
A CI/CD deployment pipeline failed midway through an upgrade because a pre-upgrade migration hook timed out. All subsequent automated builds failed with:
`Error: UPGRADE FAILED: another operation (install/upgrade/rollback) is in progress`.

### Root Cause
Helm stores the release status in a Kubernetes Secret. When the client or runner was abruptly terminated, the release remained stuck in `status: pending-upgrade`. Helm refused to execute any further operations to prevent corruption.

### Remediation
1. Located the failed release Secret:
   `kubectl get secrets -n prod -l name=my-app,status=pending-upgrade`
2. Safely rolled back to the previous successful revision:
   `helm rollback my-app <previous-good-revision> -n prod`
3. Configured `--timeout 10m` and `--atomic` in CI/CD so failed upgrades rollback automatically!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - OCI Registries](./06-OCI-Chart-Registries-and-Enterprise-Distribution.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
