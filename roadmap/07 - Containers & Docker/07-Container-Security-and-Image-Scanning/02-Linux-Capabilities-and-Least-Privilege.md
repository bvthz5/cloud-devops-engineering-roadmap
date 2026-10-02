# 02 - Linux Capabilities and Least Privilege

## 1. Deconstructing Linux Root Privileges

In traditional Unix, permissions are binary: either a process is unprivileged (UID > 0) or all-powerful `root` (UID 0).
Linux **Capabilities** break root privileges into ~40 discrete permissions:
- `CAP_NET_BIND_SERVICE`: Bind to privileged ports below 1024.
- `CAP_CHOWN`: Change file ownership.
- `CAP_SYS_ADMIN`: Overly broad "super-capability" (mounting filesystems, cgroups, namespaces).

---

## 2. Production Hardening: Drop ALL Capabilities

The golden security standard for production containers is dropping all capabilities by default, then selectively whitelisting only what is strictly required:

```bash
docker run -d \
  --name hardened_web \
  --cap-drop=ALL \
  --cap-add=NET_BIND_SERVICE \
  -p 80:80 \
  nginx:alpine
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Container Threat Modeling](./01-Container-Threat-Modeling-and-Escape-Vectors.md) | [README](./README.md) | [03 - Seccomp & AppArmor Profiles](./03-Seccomp-and-AppArmor-LSM-Profiles.md) |
