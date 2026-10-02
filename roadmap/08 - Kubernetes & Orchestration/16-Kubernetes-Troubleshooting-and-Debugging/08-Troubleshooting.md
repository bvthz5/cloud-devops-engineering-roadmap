# 08 - Troubleshooting & Diagnostic Runbooks

## Complete Kubernetes Diagnostic Cheat Sheet

```bash
# 1. Pod crashing
kubectl logs <pod> --previous -c <container>
kubectl describe pod <pod>

# 2. Node failing
kubectl describe node <node>
sudo journalctl -u kubelet -e -n 50

# 3. DNS failing
kubectl exec -it <pod> -- nslookup kubernetes.default

# 4. Live interactive triage on distroless pod
kubectl debug -it <pod> --image=nicolaka/netshoot
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
