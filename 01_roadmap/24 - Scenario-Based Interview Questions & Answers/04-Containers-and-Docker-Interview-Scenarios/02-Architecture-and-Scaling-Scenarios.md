# Containers & Docker Interview Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Zombie Processes Accumulating Inside Containers Causing Failure

### 🚨 The Production Scenario
A container running a Python microservice that forks worker subprocesses gradually slows down and eventually stops spawning new tasks.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
When a container starts, the application entrypoint runs as PID 1 inside the container's PID namespace. Standard Linux init systems (like systemd) reap orphaned zombie processes when their parent terminates. If the user's application binary runs as PID 1 and lacks a built-in `SIGCHLD` reaper, orphaned children remain zombies indefinitely.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Inspect container processes via `docker top <container>`.
- Step 2: Check for multiple `<defunct>` zombie processes.
- Step 3: Run container with Docker's built-in init flag: `docker run --init`.
- Step 4: Embed Tini (`/sbin/tini --`) or `dumb-init` as the `ENTRYPOINT` in the Dockerfile.
- Step 5: Ensure application handles `SIGTERM` signals properly for graceful shutdown.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# View live process tree inside container namespace to detect zombie defunct processes
docker top <CONTAINER_ID> -ef

# Launch container with Docker built-in lightweight init process (Tini) as PID 1
docker run --init -d myapp:latest

# Verify Dockerfile includes an explicit init binary entrypoint
grep -i 'tini' Dockerfile

# Confirm PID 1 is occupied by init and child processes are reaped
docker exec <CONTAINER_ID> ps aux

# Test that container responds to graceful shutdown signals within 10 seconds
docker kill -s SIGTERM <CONTAINER_ID>

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Containers isolate PIDs; whatever binary you run in `ENTRYPOINT` becomes PID 1. If that binary is not designed as an init system, it will not reap orphaned child processes (`SIGCHLD`), leaking PID table slots until the container fails. I resolve this by configuring `Tini` as the container's `ENTRYPOINT` or running with `docker run --init`, ensuring orphaned processes are instantly reaped and `SIGTERM` signals are forwarded properly."

---

## 📌 Scenario 5: Rootless Container Cannot Bind to Privileged Ports 80 and 443

### 🚨 The Production Scenario
For security hardening, an Nginx container is modified to run as a non-root user (`USER 1001`), but fails on startup with `nginx: [emerg] bind() to 0.0.0.0:80 failed (13: Permission denied)`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
In Linux, ports below 1024 are privileged ports restricted to the root user (`UID 0`). Running a container as an unprivileged user prevents the kernel socket bind operation unless explicit Linux capabilities or kernel sysctls are granted.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Shift internal listening port from 80/443 to non-privileged ports (e.g. 8080/8443) in Nginx config.
- Step 2: Map host port 80 to container port 8080: `docker run -p 80:8080`.
- Step 3: Alternatively, grant the specific capability `CAP_NET_BIND_SERVICE` to the binary or container.
- Step 4: Tune kernel sysctl `net.ipv4.ip_unprivileged_port_start=0` if rootless low-port binding is required.
- Step 5: Verify container runs securely without root privileges while receiving traffic.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Configure service to listen on non-privileged port (>1024)
sed -i 's/listen 80;/listen 8080;/g' /etc/nginx/conf.d/default.conf

# Expose external port 80 while container executes as unprivileged UID 1001
docker run -d -p 80:8080 --user 1001:1001 my-nginx:latest

# Grant Linux capability to allow unprivileged binary to bind low ports
setcap 'cap_net_bind_service=+ep' /usr/sbin/nginx

# Grant NET_BIND_SERVICE capability explicitly to Docker container
docker run --cap-add=NET_BIND_SERVICE --user 1001 -p 80:80 my-nginx:latest

# Lower unprivileged port threshold to port 80 via kernel sysctl
sysctl -w net.ipv4.ip_unprivileged_port_start=80

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Running containers as non-root is a critical security mandate, but Linux forbids non-root users from binding to ports under 1024. The cleanest industry solution is configuring the application to listen on high ports like 8080 or 8443 inside the container, and using the container runtime or Kubernetes Service to map external port 80 to internal 8080. If port 80 is strictly required internally, I inject `CAP_NET_BIND_SERVICE`."

---

## 📌 Scenario 6: Container Cannot Resolve External DNS or Cloud Metadata Service

### 🚨 The Production Scenario
A container running inside Docker cannot connect to AWS S3 or resolve external domain names, while the underlying Linux host has full internet connectivity.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Docker daemon creates a bridge network (`docker0`) with iptables NAT rules (`MASQUERADE`). If IP forwarding is disabled in the Linux kernel (`net.ipv4.ip_forward=0`), or if local firewalls (UFW/firewalld) drop bridge forwarding traffic, container packets never leave the host.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check host IP forwarding setting: `sysctl net.ipv4.ip_forward`.
- Step 2: Inspect iptables forward chain policy: `iptables -L FORWARD -n -v`.
- Step 3: Test DNS inside the container using `docker run --rm alpine nslookup google.com`.
- Step 4: Enable kernel IP forwarding permanently in `/etc/sysctl.conf`.
- Step 5: Adjust UFW/firewalld to allow Docker bridge forwarding: `DEFAULT_FORWARD_POLICY="ACCEPT"`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check if Linux kernel allows packet forwarding across network interfaces
sysctl net.ipv4.ip_forward

# Enable IP forwarding immediately in running kernel
sudo sysctl -w net.ipv4.ip_forward=1

# Verify Docker outbound NAT masquerade rule exists on physical interface
sudo iptables -t nat -L POSTROUTING -n -v | grep MASQUERADE

# Test raw IP egress connectivity from within container network namespace
docker run --rm busybox ping -c 3 8.8.8.8

# Test DNS resolution from within container network namespace
docker run --rm busybox nslookup google.com

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When a host can access the internet but its containers cannot, the failure is almost always kernel IP forwarding or iptables bridge drops. Docker requires `net.ipv4.ip_forward=1` to route packets from `docker0` to the physical NIC via NAT masquerade. I verify IP forwarding with `sysctl`, inspect the `iptables` FORWARD chain, and ensure local firewall utilities like UFW do not drop Docker bridge traffic."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Containers & Docker Interview Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Containers & Docker Interview Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

