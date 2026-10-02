# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Nginx Reload Storm

### Incident Summary
During an automated scale-testing exercise where 50 microservices were deploying 500 pods per minute, Ingress-Nginx CPU spiked to 100%, and incoming client connections timed out with 504 Gateway Timeout.

### Root Cause
An older version of Ingress-Nginx was running without dynamic Lua endpoint routing. Each time a pod scaled, the controller generated a new configuration file and executed `nginx -s reload`. Spawning thousands of worker processes exhausted the Linux process table (`PID limits`) and dropped all in-flight TCP handshakes.

### Remediation
1. Upgraded Ingress-Nginx to v1.8+ enabling Lua shared memory endpoint updates.
2. Enabled `worker-shutdown-timeout: 300s` and adjusted `worker_rlimit_nofile` to 100,000.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Ingress Security & WAF](./06-Ingress-Security-Rate-Limiting-and-ModSecurity-WAF.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
