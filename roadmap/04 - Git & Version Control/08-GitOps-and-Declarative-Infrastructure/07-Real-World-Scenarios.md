# 07 - GitOps: Real-World Production Scenarios

## Scenario 1: The Self-Healing CrashLoop during Incident Triage

### Incident Summary
During a Sev-1 database outage, an on-call SRE manually executed `kubectl edit deployment auth-service` to double memory limits from 2Gi to 4Gi. Within 30 seconds, the pods crashed again. The SRE was shocked to see memory limits reset back to 2Gi!

### Root Cause
Argo CD was configured with `selfHeal: true`. When the SRE modified live memory limits via `kubectl`, Argo CD flagged it as an out-of-band drift violation and immediately reapplied the 2Gi limit declared in Git!

### SRE Lessons Learned
1. During emergencies, temporarily disable automated sync via CLI:
   ```bash
   argocd app set auth-service --sync-policy manual
   ```
2. After the incident is stabilized, commit the 4Gi change into Git and re-enable automated self-healing.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Drift Detection](./06-Drift-Detection-Self-Healing-and-Rollbacks.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
