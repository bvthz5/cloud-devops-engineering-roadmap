# 09 — Kernel Networking, Sockets, Security, Namespaces, and Cgroups

---

## 1. Kernel Networking & The TCP/IP Stack

The Linux kernel contains an enterprise-grade network stack responsible for packet transmission, routing, protocol framing, and firewalling.

```text
User Application: HTTP Request / JSON Payload
       │
       ▼ send() / write()
Network Socket Layer: Socket Inode & Buffer Queue
       │
       ▼ Transport Layer: TCP (Sequence #, Acks, Retransmissions)
TCP Control Block (TCP State Machine)
       │
       ▼ Network Layer: IPv4 / IPv6 (Routing, TTL, Fragmentation)
Netfilter Hooks (PREROUTING, INPUT, FORWARD, OUTPUT, POSTROUTING)
       │
       ▼ Data Link Layer: Ethernet Framing & ARP
qdisc (Queueing Discipline / Traffic Control)
       │
       ▼ Device Driver (e.g., e1000e, ena, virtio_net)
Ring Buffers (DMA Transfer to NIC) ──► Physical Network Cable / Fiber
```

### The `sk_buff` (Socket Buffer)
In the kernel, every network packet is encapsulated in a data structure named **`struct sk_buff`**. The packet travels through the protocol layers without copying the underlying packet payload—each layer simply advances or rewrites head/tail pointers in the `sk_buff`.

---

## 2. Network Sockets & The Socket API

A **Socket** is the standard programmatic abstraction representing an endpoint for two-way network communication.

### TCP Server Lifecycle System Calls
```text
Server Workflow:                      Client Workflow:
1. socket()  ── Creates endpoint
2. bind()    ── Binds to IP & Port
3. listen()  ── Marks as passive queue
4. accept()  ── Blocks until client connects
       ▲                                 1. socket()
       │ ◄────── TCP 3-Way Handshake ──── 2. connect()
5. read()    ◄── Exchanges Data ──────── 3. write()
6. write()   ─── Exchanges Data ────────► 4. read()
7. close()   ◄── TCP 4-Way Teardown ─── 5. close()
```

---

## 3. Kernel Security: Linux Security Modules (LSM)

Traditional Unix relies on **Discretionary Access Control (DAC)**: if a user owns a file, they can set whatever permissions (`chmod 777`) they want. If a root process is compromised, the attacker gains full control of the entire server.

**Linux Security Modules (LSM)** implement **Mandatory Access Control (MAC)**: the kernel enforces strict security labels regardless of user permissions. Even if an attacker compromises a service running as `root`, the LSM confines the attacker to only what the security policy permits.

| LSM Framework | Primary Distributions | Configuration Mechanism | Behavior |
| :--- | :--- | :--- | :--- |
| **SELinux** | RHEL, CentOS, Fedora, Rocky | Inode labels (`type`, `role`, `user`) | Complex, granular, label-based (`system_u:object_r:httpd_sys_content_t`) |
| **AppArmor** | Ubuntu, Debian, SUSE | Path-based text profiles in `/etc/apparmor.d/` | Simple, path-based (`/var/www/html/ r,`) |

```bash
# Check SELinux status
sestatus

# Check AppArmor status
sudo aa-status
```

---

## 4. Users, Groups, and File Permissions

### Permission Bits Breakdown
Every file inode stores 9 basic permission bits plus 3 special bits:

```text
Special Bits: [ SUID (4) ] [ SGID (2) ] [ Sticky Bit (1) ]
User (Owner): [ Read (4) ] [ Write (2) ] [ Execute (1) ]
Group:        [ Read (4) ] [ Write (2) ] [ Execute (1) ]
Others:       [ Read (4) ] [ Write (2) ] [ Execute (1) ]
```

### Special Permission Bits:
1. **SUID (Set User ID - `4000` / `u+s`):** When executed, the binary runs with the privileges of the file owner rather than the user executing it (e.g., `/usr/bin/passwd` runs as root to update `/etc/shadow`).
2. **SGID (Set Group ID - `2000` / `g+s`):** On directories, newly created files automatically inherit the parent directory's group ownership rather than the user's primary group. Essential for shared team directories.
3. **Sticky Bit (`1000` / `+t`):** On directories, prevents users from deleting or renaming files owned by others (e.g., `/tmp`).

### POSIX Access Control Lists (ACLs)
Standard permissions only support one user and one group. POSIX ACLs allow granting access to arbitrary users:
```bash
# Grant user 'devops' read and write access to /var/log/audit.log
sudo setfacl -m u:devops:rw /var/log/audit.log

# View file ACLs
getfacl /var/log/audit.log
```

---

## 5. Linux Capabilities

Traditionally, Unix permissions were binary: you were either unprivileged (`UID != 0`) or had god-mode access (`UID == 0` / `root`).

**Linux Capabilities** break down root privileges into distinct, granular units:

| Capability | Purpose | Docker / Container Use Case |
| :--- | :--- | :--- |
| **`CAP_NET_BIND_SERVICE`** | Bind to privileged ports (< 1024) | Run Nginx on port 80 as non-root user |
| **`CAP_NET_ADMIN`** | Modify network interfaces, iptables, routing | VPN containers, Cilium CNI plugins |
| **`CAP_SYS_ADMIN`** | "The new root" — mounts filesystems, loads modules | Strongly avoided in containers; gives near-root host escape |
| **`CAP_CHOWN`** | Change file ownership | Package managers during container build |
| **`CAP_SETUID`** | Switch arbitrary process UIDs | Web servers changing worker pool UID |

### Dropping and Adding Capabilities
```bash
# Grant a non-root binary permission to bind to port 80
sudo setcap 'cap_net_bind_service=+ep' /usr/local/bin/my-web-app

# Inspect capabilities of an executable
getcap /usr/local/bin/my-web-app
```

---

## 6. Linux Namespaces: The Foundation of Containers

> **CONTAINER DEFINITION:** A container is NOT a virtual machine. A container is a standard Linux process isolated using **Linux Namespaces** and resource-constrained using **Control Groups (cgroups)**.

A **Namespace** wraps a global system resource in an abstraction that makes it appear to the processes within the namespace that they possess their own isolated instance of that resource.

```text
+-------------------------------------------------------------+
|               The 7 Core Linux Namespaces                   |
+-------------------------------------------------------------+
| Namespace | Flag              | What It Isolates             |
+-----------+-------------------+------------------------------+
| **PID**   | `CLONE_NEWPID`    | Process IDs (App sees PID 1) |
| **NET**   | `CLONE_NEWNET`    | Network interfaces, routing  |
| **MNT**   | `CLONE_NEWNS`     | Mount points, filesystems    |
| **IPC**   | `CLONE_NEWIPC`    | Shared memory, semaphores    |
| **UTS**   | `CLONE_NEWUTS`    | Hostname and domain name     |
| **USER**  | `CLONE_NEWUSER`   | UIDs/GIDs (Root inside =     |
|           |                   | unprivileged user on host)   |
| **CGROUP**| `CLONE_NEWCGROUP` | Root directory of cgroup     |
+-------------------------------------------------------------+
```

### Manual Namespace Creation with `unshare`
```bash
# Launch a completely isolated shell with its own PID and Mount namespace
sudo unshare --fork --pid --mount-proc bash
```

---

## 7. Control Groups (cgroups): Resource Metering & Limiting

While Namespaces provide **Isolation** (what a process can *see*), **cgroups** provide **Metering & Enforcement** (how much resource a process can *use*).

### What cgroups Control
- **CPU:** Limits CPU shares, quotas, and bandwidth (e.g., maximum 2 cores).
- **Memory:** Enforces maximum RAM limits and triggers container OOM killer if breached.
- **Block I/O:** Limits read/write IOPS and megabytes per second on disk devices.
- **PIDs:** Prevents fork-bombs by capping the total number of child processes allowed.

```text
Host Physical Memory: 64 GB
  │
  ├── cgroup: /docker/container-nginx (Limit: 512 MB)
  │    └── Process: nginx (Killed by OOM killer if it hits 512 MB)
  │
  └── cgroup: /docker/container-redis (Limit: 4 GB)
```

In the next modern kernel file, we will explore the revolution of **cgroups v2** and **eBPF** in depth.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Environment Variables Daemons and Device Drivers](./08-Environment-Variables-Daemons-and-Device-Drivers.md) | [README](./README.md) | [10 - Init Systems systemd and Boot Sequence](./10-Init-Systems-systemd-and-Boot-Sequence.md) |
