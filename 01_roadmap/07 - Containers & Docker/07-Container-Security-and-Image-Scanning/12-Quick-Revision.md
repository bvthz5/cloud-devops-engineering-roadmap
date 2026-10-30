# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Production Hardened Docker Run Baseline
docker run -d \
  --user 10001:10001 \
  --security-opt no-new-privileges:true \
  --cap-drop=ALL \
  --read-only \
  --tmpfs /tmp:rw,noexec \
  myapp:v1.0

# Trivy CI Scan Command
trivy image --severity HIGH,CRITICAL --exit-code 1 myapp:v1.0
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (08-Container-Registries-DockerHub-ECR-GHCR) →](../08-Container-Registries-DockerHub-ECR-GHCR/01-OCI-Distribution-Spec-and-Registry-HTTP-API.md) |
