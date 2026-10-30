# 05 - Non-Root Users and Least Privilege Execution

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to container security boundaries, the peril of running as UID 0, user namespace remapping, least privilege enforcement, and non-root authoring standards.

---

## 🏛️ 1. What Is Container User Context & Why Is Root Execution Dangerous?

In Linux operating systems, the superuser account is identified by User Identifier **UID 0** (traditionally named `root`). UID 0 possesses unrestricted administrative authority across the operating system kernel, granting bypass permissions over file access controls, network interfaces, and process signals.

When a container image does not specify a `USER` instruction, Docker defaults to executing all processes as UID 0. 

A widespread, dangerous misconception among software developers is assuming that "container root" is virtualized and isolated from the host. In reality, **containers share the host operating system kernel directly**. A process running as UID 0 inside a container is evaluated by the Linux kernel as UID 0. While Linux namespaces restrict what resources that process can *see*, its security authority remains root.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   THE PERIL OF CONTAINER ROOT EXECUTION                │
└────────────────────────────────────────────────────────────────────────┘

 [ Container Filesystem ]                         [ Host Linux Kernel ]
 Application Process (UID 0: root) ─────────────► Shares Host Kernel!
           │
           │ (Attacker achieves Remote Code Execution)
           ▼
 Exploit Container Escape (e.g., CVE-2024-21626 / Leaked Docker Socket)
           │
           ▼
 Attacker Emerges on Host as: UID 0 (root)! 💥 FULL HOST TAKEOVER!
```

### Critical Escape Vectors of Root Containers:
1. **Host Filesystem Mounts:** If a host directory or socket (such as `/var/run/docker.sock` or `/etc`) is mounted into a container running as root, the container process can overwrite host configuration files, create backdoors, or command the Docker daemon to spawn privileged host containers.
2. **Kernel Vulnerabilities:** If an attacker exploits a Linux kernel vulnerability (such as Dirty COW, Dirty Pipe, or runc descriptor leaks), the kernel evaluates the escaping process as UID 0, immediately granting the attacker root privileges over the entire physical or virtual host.
3. **Enterprise Compliance Failures:** Enterprise security frameworks (including CIS Docker Benchmark, SOC 2, and PCI-DSS) strictly mandate that containerized production workloads run as unprivileged users.

---

## ⚙️ 2. How to Implement Non-Root Users in Dockerfiles

Adhering to the **Principle of Least Privilege** requires creating an explicit, unprivileged system user and group within the container image and instructing Docker to switch to that user before executing the application.

### 2.1 Implementing Non-Root on Debian / Ubuntu Base Images:
Debian-based distributions utilize the `groupadd` and `useradd` commands:

```dockerfile
FROM python:3.11-slim-bookworm

# 1. Create a dedicated system group and user with explicit UID/GID
RUN groupadd -g 10001 appgroup && \
    useradd -u 10001 -g appgroup -s /sbin/nologin -M -d /app appuser

WORKDIR /app

# 2. Copy application files with ownership assigned directly to appuser
COPY --chown=10001:10001 requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY --chown=10001:10001 . .

# 3. Create writable directories needed for local logs or temporary files
RUN mkdir -p /app/logs && chown -R 10001:10001 /app/logs

# 4. Switch user context to unprivileged UID
USER 10001:10001

EXPOSE 8080
CMD ["python", "app.py"]
```

### 2.2 Implementing Non-Root on Alpine Linux Base Images:
Alpine Linux utilizes BusyBox utilities, requiring the `addgroup` and `adduser` commands:

```dockerfile
FROM node:20-alpine

# 1. Create unprivileged group and user in Alpine syntax
RUN addgroup -g 10001 -S appgroup && \
    adduser -u 10001 -S appuser -G appgroup -s /sbin/nologin

WORKDIR /app

# 2. Copy artifacts with ownership assigned to appuser
COPY --chown=10001:10001 package*.json ./
RUN npm ci --only=production

COPY --chown=10001:10001 . .

# 3. Switch user context
USER 10001:10001

EXPOSE 3000
CMD ["node", "server.js"]
```

---

## 🔬 3. Best Practices for Non-Root Authoring

1. **Always Use Numeric UIDs (`USER 10001:10001`):** While Docker allows specifying usernames as strings (`USER appuser`), enterprise orchestrators like Kubernetes cannot resolve usernames if `/etc/passwd` is missing (such as in Distroless images) or when verifying admission policies (`runAsNonRoot: true`). Declaring explicit numbers guarantees consistent policy enforcement across all container platforms.
2. **Avoid Default UID 1000:** The first standard user created on a Linux host or developer workstation is typically assigned UID 1000. If a container running as UID 1000 writes to a host bind mount, file ownership conflicts occur. Using a dedicated high numeric ID (e.g., `10001`) prevents accidental host user overlap.
3. **Use `--chown` on `COPY` Instructions:** In older Dockerfiles, developers ran `COPY . /app` followed by `RUN chown -R appuser:appgroup /app`. This doubled image size because the second layer duplicated every file to update metadata. The `--chown` flag on `COPY` assigns correct ownership at injection time with zero layer overhead.
4. **Disable Login Shells (`-s /sbin/nologin`):** The application user should be a service account with no interactive login shell, preventing shell access even if credentials are breached.

---

## 🌐 4. Binding Low Network Ports (< 1024) Without Root

Historically in Unix systems, network ports below 1024 (such as port 80 for HTTP and port 443 for HTTPS) were designated as "privileged ports" that could only be bound by processes possessing the `CAP_NET_BIND_SERVICE` capability, traditionally reserved for root.

In modern container architectures, there are two primary methods to run unprivileged containers on web ports:

### Method 1: Listen on High Ports and Map with Docker (Recommended)
Configure your application server to listen on port `8080` internally, and use Docker port publishing or a reverse proxy to bind host port 80:
```bash
docker run -d -p 80:8080 my-nonroot-image
```

### Method 2: Kernel Sysctl Unprivileged Port Start
In modern Linux kernels (version 4.11 and newer) and Docker Engine 20.10+, Docker automatically sets the kernel sysctl parameter `net.ipv4.ip_unprivileged_port_start = 0` inside container network namespaces. This allows unprivileged non-root users inside the container to bind ports 80 and 443 directly without requiring root privileges.

---

## 🛡️ 5. Advanced Defense: User Namespace Remapping (`userns-remap`)

For organizations running multi-tenant hosts where legacy container images cannot be easily modified to run as non-root, Docker provides **User Namespace Remapping (`userns-remap`)**.

When user namespace remapping is enabled in `/etc/docker/daemon.json`, the Docker daemon creates a translation table between container UIDs and host UIDs:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   USER NAMESPACE REMAPPING ARCHITECTURE                │
└────────────────────────────────────────────────────────────────────────┘

 Container View:
 Container Process thinks it is running as: UID 0 (root)

 Linux Kernel Translation:
 Translated on the Host Operating System as: UID 165536 (Unprivileged!)
```

If an attacker achieves a full container escape from a remapped container, the host kernel evaluates their process as UID 165536. Because UID 165536 has zero administrative privileges on the host filesystem, the attacker cannot read `/etc/shadow`, cannot write to `/bin`, and cannot compromise the underlying machine.

---

## 📋 6. Summary and Navigation

Enforcing least privilege by running containers as non-root users forms an essential security baseline in modern DevOps engineering. By explicitly creating unprivileged service accounts, utilizing numeric UIDs, and eliminating root execution from production images, engineering teams eliminate major container escape vectors and protect mission-critical cloud infrastructure.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Distroless, Alpine, and Scratch Base Images](./04-Distroless-Alpine-and-Scratch-Base-Images.md) | [Index](../../../README.md) | [06 - BuildKit Advanced Features and Cache Mounts →](./06-BuildKit-Advanced-Features-and-Cache-Mounts.md) |
