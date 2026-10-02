# 09 - Interview Questions & Architectural Scenarios

### Q1: How do you debug a Pod in CrashLoopBackOff that crashes within 0.5 seconds of booting?
**Answer:**
1. Check previous container logs: `kubectl logs <pod> --previous`.
2. Inspect exit code: `kubectl describe pod <pod>` (look for `Last State: Terminated, Exit Code: X`).
3. Override entrypoint command via temporary patch or debug container to keep the container alive:
   `kubectl patch deployment my-app -p '{"spec":{"template":{"spec":{"containers":[{"name":"app","command":["sleep","3600"]}]}}}}'`
   Then `kubectl exec -it` inside to inspect files and environment variables.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
