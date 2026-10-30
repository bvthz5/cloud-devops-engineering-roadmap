# 09 - Interview Questions & Architectural Scenarios

### Q1: How does Traefik's service discovery architecture fundamentally differ from traditional Nginx?
**Answer**: Traditional Nginx relies on static configuration files; adding or removing backend nodes requires modifying configuration files and executing a reload (`nginx -s reload`). Traefik connects directly to orchestrator event streams (Docker API, Kubernetes API) as **Providers**. When a container spawns or dies, Traefik updates internal routing data structures dynamically in memory with zero reloads and zero downtime.

### Q2: What are the security risks of mounting `/var/run/docker.sock` in Traefik, and how do you mitigate them?
**Answer**: Mounting the Docker socket grants full control over the host's Docker daemon, allowing anyone with access to the container to spawn privileged containers and mount host filesystems. Mitigation: 1) Use an unprivileged read-only Docker socket proxy (`docker-socket-proxy`) that restricts API endpoints to read-only container listings, 2) Mount with `:ro`.

### Q3: Why is `acme.json` required to have 0600 permissions in Traefik?
**Answer**: `acme.json` stores the private keys of all generated SSL/TLS certificates and the ACME account private key in plaintext JSON. Traefik enforces POSIX 0600 permissions at startup to ensure no other user or unprivileged process on the system can read private keys.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
