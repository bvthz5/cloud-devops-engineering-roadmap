# 05 - Buildah: Scriptable Daemonless Image Building

## 1. Building Containers from Plain Bash Scripts

**Buildah** builds OCI container images without requiring a Dockerfile or a running daemon:

```bash
#!/usr/bin/env bash
set -euo pipefail

# 1. Start from base image
container=$(buildah from alpine:latest)

# 2. Mount container rootfs directly to host directory!
mountpoint=$(buildah mount $container)

# 3. Copy files and execute commands inside rootfs
cp ./app.py "$mountpoint/app.py"
buildah run $container apk add --no-cache python3

# 4. Set container metadata
buildah config --entrypoint '["python3", "/app.py"]' $container
buildah config --author "DevOps Team" $container

# 5. Commit into OCI image
buildah commit $container myregistry.com/app:v1.0
buildah rm $container
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Systemd Integration and Podman Quadlets](./04-Systemd-Integration-and-Podman-Quadlets.md) | [Index](../../../README.md) | [06 - Skopeo Remote Registry Operations Without Pulling →](./06-Skopeo-Remote-Registry-Operations-Without-Pulling.md) |
