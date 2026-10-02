# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Docker Daemon Crash Multi-Tenant Outage

### Context & Incident
A company ran 80 production containers on an enterprise Linux server using Docker. A rogue monitoring script crashed the `dockerd` daemon. Immediately, all 80 containers ceased routing traffic and dropped user sessions.

### Architectural Comparison with Podman
Under Podman, each container runs under an independent lightweight **`conmon`** (container monitor) process. There is no central daemon. If the Podman CLI exits or crashes, running containers continue executing with 100% uptime!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Skopeo: Remote Registry Operations](./06-Skopeo-Remote-Registry-Operations-Without-Pulling.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
