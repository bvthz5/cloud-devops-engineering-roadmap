# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Debugging `client-side throttling, qps exceeded`

When controllers execute rapid batch operations against large clusters, `client-go` throttles requests client-side before sending them over the network.

```text
Warning: Waited 1.253s due to client-side throttling, not priority and fairness, request: GET:https://...
```

### Fix: Tune Client QPS and Burst
```go
config, err := rest.InClusterConfig()
// Default is QPS=5, Burst=10 (too low for enterprise controllers)
config.QPS = 100
config.Burst = 200
```

---

## 2. RBAC Authorization Debugging

When a controller fails with `is forbidden: User "system:serviceaccount:..." cannot get resource...`:

```bash
# Test RBAC permissions using auth can-i as the controller's ServiceAccount
kubectl auth can-i create deployments \
  --as=system:serviceaccount:my-operator-system:my-operator-controller-manager \
  -n default
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
