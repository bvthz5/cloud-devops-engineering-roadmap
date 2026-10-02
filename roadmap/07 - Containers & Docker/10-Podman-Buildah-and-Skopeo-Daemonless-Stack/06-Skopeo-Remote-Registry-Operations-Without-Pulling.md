# 06 - Skopeo: Remote Registry Operations Without Pulling

## 1. Zero-Disk Image Operations

Traditionally, copying an image between two registries requires `docker pull` (downloading gigabytes to local disk) followed by `docker push`.
**Skopeo** performs operations **directly over the network between remote registries**:

```bash
# 1. Inspect remote image manifest and metadata without downloading layers
skopeo inspect docker://docker.io/library/nginx:latest

# 2. Copy image directly from Docker Hub to AWS ECR over the network
skopeo copy \
  docker://docker.io/library/nginx:latest \
  docker://123456789012.dkr.ecr.us-east-1.amazonaws.com/nginx:latest

# 3. Delete an image tag from a remote registry
skopeo delete docker://myregistry.com/app:old-tag
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Buildah: Scriptable Builds](./05-Buildah-Scriptable-Daemonless-Image-Building.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
