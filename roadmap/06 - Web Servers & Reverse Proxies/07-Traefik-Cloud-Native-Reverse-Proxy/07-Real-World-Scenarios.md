# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Docker Socket Exposure Vulnerability

### Context & Incident
A platform engineer ran Traefik inside a Docker container with `/var/run/docker.sock` mounted with read-write permissions. An application container behind Traefik was exploited via a remote code execution (RCE) flaw. Attackers breached the internal Docker network, accessed Traefik's container, and gained root access to the entire host OS via the Docker socket.

### Root Cause
Mounting `/var/run/docker.sock` directly gives full root daemon control over the host.

### Hardened Architecture: Docker Socket Proxy
Never expose the raw Docker socket directly to Traefik! Deploy an intermediate **Docker Socket Proxy** (`tecnativa/docker-socket-proxy`) that grants read-only access strictly to container events:

```yaml
services:
  docker-proxy:
    image: tecnativa/docker-socket-proxy
    environment:
      - CONTAINERS=1 # Read-only container info
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"

  traefik:
    image: traefik:v3.0
    command:
      - "--providers.docker.endpoint=tcp://docker-proxy:2375"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Observability & Dashboard](./06-Observability-Tracing-and-Dashboard.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
