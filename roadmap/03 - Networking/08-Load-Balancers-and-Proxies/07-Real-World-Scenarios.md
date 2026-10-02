# 07 - Load Balancers & Proxies: Real-World Production Scenarios

## Scenario 1: The Rolling Deployment 502 Cascade

### Incident Summary
During a continuous delivery deployment to a Kubernetes cluster, users reported intermittent `502 Bad Gateway` errors lasting 15 to 45 seconds during every pod rollout.

### Root Cause
1. Kubernetes sent a `SIGTERM` to the old Pod.
2. The Pod immediately stopped accepting connections and shut down its web server.
3. However, updating the upstream endpoints in NGINX Ingress took 1.5 seconds.
4. NGINX continued sending incoming user traffic to the dead Pod, producing 502 errors!

### Resolution
Implemented a `preStop` lifecycle hook in the Kubernetes Deployment manifest:
```yaml
lifecycle:
  preStop:
    exec:
      command: ["/bin/sh", "-c", "sleep 5"]
```
This delays container shutdown by 5 seconds, giving ingress controllers ample time to deregister the pod endpoint before the server stops listening.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Sticky Sessions](./06-Session-Persistence-and-Sticky-Sessions.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
