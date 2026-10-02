# 06 - OCI Chart Registries and Enterprise Distribution

## 1. Native OCI Chart Packaging (Helm v3.8+)

Modern Helm distributes charts as standard **OCI Artifacts** alongside container images in Amazon ECR, Google Artifact Registry, or Harbor:

```bash
# 1. Package chart into .tgz archive
helm package ./payment-service

# 2. Login to OCI Registry
helm registry login my-registry.azurecr.io

# 3. Push chart as OCI artifact
helm push payment-service-1.4.0.tgz oci://my-registry.azurecr.io/helm-charts

# 4. Install directly from OCI URL!
helm install payment-service oci://my-registry.azurecr.io/helm-charts/payment-service --version 1.4.0
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Helm Hooks](./05-Helm-Hooks-and-Lifecycle-Management.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
