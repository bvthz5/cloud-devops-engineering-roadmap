# 02 - Kubernetes Secrets: Types and Base64 Encoding Realities

## 1. The Myth: "Secrets are Encrypted"

> [!WARNING]
> **Base64 is NOT encryption!** Base64 is purely an encoding scheme used to safely transmit binary data in ASCII format. Anyone with read access to the Secret resource or access to `etcd` can instantly decode it:
> `echo "c3VwZXJzZWNyZXQ=" | base64 -d` ──► `supersecret`

---

## 2. Standard Secret Types

| Secret Type | Purpose | Required Keys |
|---|---|---|
| **`Opaque`** | Arbitrary user-defined confidential data | Custom keys (e.g. `password`, `api_token`) |
| **`kubernetes.io/tls`** | X.509 TLS certificate and private key | `tls.crt`, `tls.key` |
| **`kubernetes.io/dockerconfigjson`** | Container registry authentication credentials | `.dockerconfigjson` |
| **`kubernetes.io/service-account-token`** | ServiceAccount JWT token for API authentication | `token`, `ca.crt`, `namespace` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - ConfigMap Creation](./01-ConfigMap-Creation-Environment-and-Volume-Projections.md) | [README](./README.md) | [03 - Encryption at Rest](./03-Secret-Encryption-at-Rest-with-KMS-Providers.md) |
