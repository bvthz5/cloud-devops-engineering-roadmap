# 06 - Multi-Architecture Builds with Buildx

## 1. Cross-Platform Container Images (x86_64 and ARM64)

With the rise of AWS Graviton (ARM64) and Apple Silicon developers (M1/M2/M3), production container images must support multi-architecture manifests:

```bash
# 1. Install QEMU emulators for cross-compilation
docker run --privileged --rm tonistiigi/binfmt --install all

# 2. Create and bootstrap Buildx builder instance
docker buildx create --name multiarch-builder --use
docker buildx inspect --bootstrap

# 3. Build and push image targeting both AMD64 and ARM64 simultaneously
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t myregistry.com/app:v1.0.0 \
  --push .
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Daemon Configuration and Production Tuning](./05-Daemon-Configuration-and-Production-Tuning.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
