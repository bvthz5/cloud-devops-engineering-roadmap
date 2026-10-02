# 10 - Hands-On Practice Labs

## Lab 1: Hardened Container Security Baseline

### Objective
Launch a container with dropped capabilities, read-only rootfs, and no new privileges enabled.

### Implementation
```bash
docker run -d \
  --name hardened_app \
  --security-opt no-new-privileges:true \
  --cap-drop=ALL \
  --cap-add=NET_BIND_SERVICE \
  --read-only \
  --tmpfs /tmp:rw,noexec,nosuid \
  -p 8080:80 \
  nginx:alpine

# Verify security settings
docker inspect --format '{{.HostConfig.ReadonlyRootfs}}' hardened_app # true
docker inspect --format '{{.HostConfig.CapDrop}}' hardened_app        # [ALL]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
