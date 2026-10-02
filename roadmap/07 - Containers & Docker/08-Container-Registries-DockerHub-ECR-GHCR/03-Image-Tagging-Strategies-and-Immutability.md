# 03 - Image Tagging Strategies and Immutability

## 1. The Peril of `:latest`

Deploying `:latest` in production is a critical antipattern:
1. It is non-deterministic (what worked yesterday may fail today with zero code changes).
2. Kubernetes kubelet caches images locally; if `:latest` points to a new image, nodes that already have a cached copy will not pull the update unless `imagePullPolicy: Always` is set.
3. Rollbacks become impossible because the previous image tag cannot be referenced.

---

## 2. Production Tagging Strategy

Every production build should be tagged with **Git SHA** and **Semantic Version**:
```bash
# Primary immutable tag (Git Commit SHA)
docker tag myapp:build myregistry.com/myapp:git-7a8f9c2

# Semantic release tag
docker tag myapp:build myregistry.com/myapp:v1.4.2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Enterprise Registries ECR GHCR and Harbor](./02-Enterprise-Registries-ECR-GHCR-and-Harbor.md) | [Index](../../../README.md) | [04 - Authentication IAM Roles and Token Exchanges →](./04-Authentication-IAM-Roles-and-Token-Exchanges.md) |
