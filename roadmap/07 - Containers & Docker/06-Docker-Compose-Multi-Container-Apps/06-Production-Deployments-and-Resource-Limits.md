# 06 - Production Deployments and Resource Limits in Docker Compose

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to hardening Docker Compose for production single-host deployments, defining cgroup CPU and memory resource boundaries, configuring restart policies, and managing log rotation.

---

## 🏛️ 1. What Are Production Deployments in Docker Compose?

While large-scale distributed architectures rely on Kubernetes or Nomad for multi-node clustering, a substantial portion of enterprise software (such as internal developer portals, staging environments, data pipelines, and single-tenant appliances) is hosted on standalone Linux virtual machines.

When operating Docker Compose in production environments, default container behaviors are dangerous. By default:
- Containers have **unlimited access** to host CPU and RAM, allowing a memory leak in a single service to trigger the Linux Out-Of-Memory (OOM) killer and crash the host operating system.
- Containers **do not restart automatically** following an unexpected failure or a physical server reboot.
- Container logs write indefinitely to local JSON files on disk, leading to **100% disk utilization** and node outages.

Hardening a Docker Compose topology for production requires enforcing strict resource boundaries, defining deterministic restart policies, and establishing automated log rotation.

---

## ⚙️ 2. Memory and CPU Resource Limits via cgroups

Docker Compose allows operators to govern container resource consumption through the `deploy.resources` specification block. These declarations are translated directly by the Docker daemon into Linux **Control Groups (cgroups)** parameters.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   CGROUP RESOURCE ENFORCEMENT MODEL                    │
└────────────────────────────────────────────────────────────────────────┘

 Host Resources: 16 GB Total RAM, 4 CPU Cores
         │
         ▼
 [ Service: Backend API ]
 ├── limits:
 │   ├── memory: 1024M  ──> Linux cgroup memory.max enforced.
 │   │                      If process exceeds 1024M, kernel OOM terminates it!
 │   └── cpus: '1.5'    ──> Linux cfs_quota_us / cfs_period_us enforced.
 │                          Process can consume at most 150% of a single core.
 └── reservations:
     └── memory: 512M   ──> Minimum guaranteed RAM reservation.
```

### Detailed Resource Directives:
1. **`limits.memory`:** Sets the absolute maximum boundary of RAM the container is permitted to consume. If processes inside the container exceed this limit, the Linux kernel terminates the offending process with an **Out Of Memory (OOM) 137 exit code**.
2. **`reservations.memory`:** Configures a soft memory reservation. The Docker host ensures that the container is guaranteed at least this amount of physical RAM during scheduling.
3. **`limits.cpus`:** Configures fractional CPU limits using the Completely Fair Scheduler (CFS). A limit of `'1.5'` means the container processes can consume up to 1.5 CPU cores worth of execution time every scheduling period.
4. **`limits.pids`:** Restricts the total number of processes and threads that can be spawned inside the container. This prevents malicious attacks or runaway recursive fork loops (fork bombs) from exhausting the host process table.

---

## 🔬 3. Container Restart Policies

When a containerized process crashes or when the underlying Docker host reboots, Docker evaluates the service's **restart policy** to determine whether to restore the container:

| Restart Policy | Behavioral Working Mechanism | Production Recommendation |
| :--- | :--- | :--- |
| **`no` (Default)** | Never restarts the container under any circumstance. | Recommended only for one-off batch scripts, seeders, and migration jobs. |
| **`always`** | Restarts the container continuously, regardless of exit code. Restarts even if explicitly stopped manually after a host reboot. | Use with caution; may restart services that were intentionally taken offline. |
| **`on-failure`** | Restarts the container only if the process exits with a non-zero exit code. Supports an optional retry limit (`on-failure:5`). | Ideal for transient background workers and queue listeners. |
| **`unless-stopped`** | Restarts the container continuously unless it was explicitly stopped by an administrator using `docker stop`. Preserves stopped state across server reboots. | **Recommended enterprise default** for production services. |

---

## 📝 4. Complete Production Compose Configuration Template

The following complete configuration illustrates a production-ready web application topology with resource limits, healthchecks, restart policies, and log rotation:

```yaml
services:
  api:
    image: mycompany/production-api:v3.2.1
    container_name: production_api_service
    restart: unless-stopped
    
    # 1. cgroup Resource Boundaries
    deploy:
      resources:
        limits:
          cpus: '2.0'
          memory: 2048M
          pids: 200                  # Defends against process fork bombs
        reservations:
          cpus: '0.5'
          memory: 512M
    
    # 2. Automated Log Rotation
    logging:
      driver: "json-file"
      options:
        max-size: "20m"              # Rotate log file when it reaches 20MB
        max-file: "5"                # Retain a maximum of 5 rotated files (100MB max)
        compress: "true"             # Compress rotated archives with gzip
    
    # 3. Security Hardening
    read_only: true                  # Immutable root filesystem
    tmpfs:
      - /tmp:rw,noexec,nosuid,size=64m
    cap_drop:
      - ALL                          # Drop all 40+ Linux capabilities
    cap_add:
      - NET_BIND_SERVICE             # Add back only low-port binding privilege
    
    networks:
      - backend_network

networks:
  backend_network:
    driver: bridge
```

---

## 📊 5. Log Driver Strategies and Disk Space Protection

By default, Docker captures standard output (`stdout`) and standard error (`stderr`) from all containers and writes them to JSON log files on the host filesystem (`/var/lib/docker/containers/<id>/<id>-json.log`).

If log rotation is not explicitly defined, high-throughput applications will silently fill the host disk drive, triggering catastrophic filesystem write locks.

### Critical Logging Driver Options:
- **`max-size`:** Sets the maximum byte size a single log file can reach before it is closed and rotated (e.g., `"20m"`, `"50m"`).
- **`max-file`:** Sets the maximum number of historical rotated log files to retain before the oldest archive is deleted.
- **`compress: "true"`:** Compresses rotated log files using gzip, reducing disk storage footprint by over 80%.

---

## 📋 6. Summary and Navigation

Hardening single-host Docker Compose deployments transforms fragile development environments into robust enterprise production platforms. By strictly enforcing cgroup memory and CPU limits, configuring `restart: unless-stopped`, dropping unnecessary Linux capabilities, and activating automated log rotation, systems withstand unpredictable traffic spikes and ensure continuous service availability.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Compose Profiles and Multi-Environment Overrides](./05-Compose-Profiles-and-Multi-Environment-Overrides.md) | [Index](../../../README.md) | [07 - Real-World Scenarios →](./07-Real-World-Scenarios.md) |
