# 02 - VolumeClaimTemplates and Per-Replica Storage

## 1. How VolumeClaimTemplates Work

Unlike Deployments where all replicas share the same PVC declaration, a StatefulSet uses `volumeClaimTemplates`.
The StatefulSet controller automatically provisions a unique PVC for each Pod ordinal:
`<claim-name>-<statefulset-name>-<ordinal>`

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: elasticsearch
spec:
  serviceName: "es-cluster"
  replicas: 3
  selector:
    matchLabels:
      app: es
  template:
    metadata:
      labels:
        app: es
    spec:
      containers:
      - name: es
        image: elasticsearch:8.10.0
        volumeMounts:
        - name: es-storage
          mountPath: /usr/share/elasticsearch/data
  volumeClaimTemplates:
  - metadata:
      name: es-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "fast-nvme"
      resources:
        requests:
          storage: 250Gi
```

> [!IMPORTANT]
> When a StatefulSet is scaled down, **its PVCs are NOT automatically deleted!** This prevents catastrophic data loss. If you scale `elasticsearch` back up from 1 to 3 replicas, Pods `elasticsearch-1` and `elasticsearch-2` will re-attach to their original persistent disks!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - StatefulSet Architecture Stable Network and Storage Identity](./01-StatefulSet-Architecture-Stable-Network-and-Storage-Identity.md) | [Index](../../../README.md) | [03 - OrderedReady vs Parallel Pod Management Policies →](./03-OrderedReady-vs-Parallel-Pod-Management-Policies.md) |
