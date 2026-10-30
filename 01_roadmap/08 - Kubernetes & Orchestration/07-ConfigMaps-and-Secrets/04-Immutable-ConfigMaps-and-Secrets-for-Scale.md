# 04 - Immutable ConfigMaps and Secrets for Scale

## 1. The Watch Problem at Scale

In clusters with tens of thousands of pods, `kubelet` maintains active watch streams on every mounted ConfigMap and Secret to detect file changes and update local volume mounts.
This places massive CPU and memory load on `kube-apiserver`.

---

## 2. Enabling `immutable: true`

Setting `immutable: true` informs `kube-apiserver` that this object will never change:
1. `kubelet` terminates its watch stream, immediately reducing API server load.
2. Prevents accidental modifications or configuration drift by administrators.
3. If an update is required, applications follow a clean immutable pattern: deploy `app-config-v2` and update the Deployment manifest!

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: immutable-config-v1
immutable: true
data:
  APP_PORT: "8080"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Secret Encryption at Rest with KMS Providers](./03-Secret-Encryption-at-Rest-with-KMS-Providers.md) | [Index](../../../README.md) | [05 - External Secrets Operator ESO and Vault Sync →](./05-External-Secrets-Operator-ESO-and-Vault-Sync.md) |
