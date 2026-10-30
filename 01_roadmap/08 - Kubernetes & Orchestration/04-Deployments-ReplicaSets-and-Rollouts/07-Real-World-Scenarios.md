# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The 502 Bad Gateway Rollout Storm

### Incident Summary
Every time CI/CD rolled out a new version of the API deployment, user-facing error rates surged to 8% for 45 seconds, triggering PagerDuty Sev-1 incidents.

### Root Cause
1. Containers were shutting down immediately upon receiving `SIGTERM`, severing active TCP keep-alive connections from Nginx Ingress.
2. Ingress controller kube-proxy iptables sync took 2.5 seconds to remove the dead Pod IPs from the endpoint lists.
3. No `preStop` hook was configured to delay termination.

### Remediation
1. Implemented a `preStop` hook: `sleep 10`.
2. Handled `SIGTERM` inside the Node.js/Go backend to stop accepting new requests, while allowing existing active HTTP requests 15 seconds to finish processing before exiting.
3. Rollout errors dropped from 8% to absolute zero (0.000%).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Container Lifecycle Hooks PostStart and PreStop](./06-Container-Lifecycle-Hooks-PostStart-and-PreStop.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
