# 04 - Rootless Docker Architecture

## 1. Eliminating the Root Daemon

Traditionally, the `dockerd` daemon runs as `root`. A vulnerability in the daemon allows total host takeover.
**Rootless Docker** runs both the `dockerd` daemon and all containers inside an unprivileged user namespace:

```text
Host System (Physical Machine):
[ Unprivileged User 'devops' (UID 1000) ]
        │
        ▼ (Uses User Namespaces: subuid / subgid)
[ Rootless dockerd (Runs as UID 1000) ]
        │
        ▼
[ Container Process (Thinks it is UID 0 inside, but maps strictly to UID 1000 on host!) ]
```

Even if an attacker breaches the container and escapes, they are only an unprivileged user (`UID 1000`) on the host!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Seccomp & AppArmor Profiles](./03-Seccomp-and-AppArmor-LSM-Profiles.md) | [README](./README.md) | [05 - Static Image Vulnerability Scanning](./05-Static-Image-Vulnerability-Scanning.md) |
