# 06 - DaemonSet Update Strategies and HostPort Considerations

## 1. RollingUpdate on DaemonSets

```yaml
spec:
  updateStrategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1         # Updates one node at a time
```

---

## 2. Tolerations for Master Nodes

By default, control plane nodes have a taint (`node-role.kubernetes.io/control-plane:NoSchedule`). If your monitoring or CNI DaemonSet must run on master nodes as well:

```yaml
spec:
  template:
    spec:
      tolerations:
      - operator: Exists        # Tolerates ALL node taints!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - DaemonSet Architecture](./05-DaemonSet-Architecture-and-Node-Level-Agents.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
