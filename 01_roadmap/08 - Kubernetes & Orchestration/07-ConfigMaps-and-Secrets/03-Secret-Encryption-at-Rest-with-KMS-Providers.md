# 03 - Secret Encryption at Rest with KMS Providers

## 1. Why Encrypt etcd at Rest?

By default, etcd stores Kubernetes Secrets in plain text on the master node disk (`/var/lib/etcd`). If an attacker takes an unencrypted snapshot or accesses the disk, all cluster passwords and tokens are compromised.

---

## 2. EncryptionConfiguration Specification

To enforce hardware-grade envelope encryption via AWS KMS, Azure Key Vault, or GCP Cloud KMS:

```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - kms:
          apiVersion: v2
          name: aws-kms-provider
          endpoint: unix:///var/run/kmsplugin/socket.sock
          timeout: 3s
      - identity: {}      # Fallback to read unencrypted secrets during migration
```

```bash
# Pass configuration flag to kube-apiserver:
--encryption-provider-config=/etc/kubernetes/encryption-config.yaml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Kubernetes Secrets Types and Base64 Encoding Realities](./02-Kubernetes-Secrets-Types-and-Base64-Encoding-Realities.md) | [Index](../../../README.md) | [04 - Immutable ConfigMaps and Secrets for Scale →](./04-Immutable-ConfigMaps-and-Secrets-for-Scale.md) |
