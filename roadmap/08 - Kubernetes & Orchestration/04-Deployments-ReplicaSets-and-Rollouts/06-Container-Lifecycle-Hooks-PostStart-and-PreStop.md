# 06 - Container Lifecycle Hooks: PostStart and PreStop

## 1. The Critical Role of `preStop` in Rolling Updates

During a rolling update, the API server removes the terminating Pod from the Service endpoint list asynchronously while simultaneously sending `SIGTERM` to the container process.
Because iptables/IPVS rule updates propagate across worker nodes with a 1–3 second delay, **in-flight requests will hit a container that is already terminating, causing 502/504 errors!**

```yaml
spec:
  containers:
  - name: api
    image: my-app:v1
    lifecycle:
      preStop:
        exec:
          # Sleep for 5 seconds to let iptables updates propagate before SIGTERM!
          command: ["/bin/sh", "-c", "sleep 5"]
```

```text
API Server marks Pod Terminating ───► EndpointSlice controller updates iptables (takes ~2s)
              │
              ├──► Kubelet executes preStop hook: "sleep 5"
              │    (Container continues serving existing and delayed requests!)
              │
              └──► 5 seconds later: Kubelet sends SIGTERM to app
                   (All iptables rules are updated; zero dropped requests!)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Pausing & Scaling](./05-Pausing-Resuming-and-Scaling-Deployments.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
