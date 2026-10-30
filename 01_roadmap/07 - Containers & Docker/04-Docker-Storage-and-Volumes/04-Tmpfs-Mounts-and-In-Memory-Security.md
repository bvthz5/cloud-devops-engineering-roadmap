# 04 - Tmpfs Mounts and In-Memory Security

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to Linux `tmpfs` in-memory mounts, zero-disk trace security, protection against forensic data recovery, memory exhaustion defense, mount flags (`noexec`, `nosuid`), and hardened read-only container patterns.

---

## 🏛️ 1. What Is a tmpfs Mount & Why Is It Critical for Security?

In high-security enterprise container environments, protecting sensitive credentials (such as TLS private keys, decrypted database tokens, transient OAuth session state, and shared cryptographic materials) requires guaranteeing that those secrets **never touch persistent physical storage media** (NVMe, SSD, or HDD).

If sensitive information is written to a standard container writable layer or a host directory:
1. **Flash Memory Remanence:** Modern solid-state drives utilize internal wear-leveling algorithms. Even if a file is deleted, physical flash blocks retain deleted data until garbage collected, allowing attackers with physical or snapshot access to perform forensic recovery.
2. **Swap File Exposure:** Memory pages can be paged out to unencrypted host swap partitions on disk.
3. **Backup Contamination:** Automated disk snapshot systems (such as AWS EBS snapshots or VMware snapshots) capture the entire filesystem state, inadvertently recording plain-text credentials in backup archives.

A **`tmpfs` (temporary file system)** mount completely eliminates these attack vectors. It is a native Linux kernel filesystem backed strictly by volatile Random Access Memory (RAM). Data written to a `tmpfs` mount exists exclusively in system memory. The millisecond the container process terminates or stops, the kernel deallocates those memory pages, leaving **zero physical trace** on any disk.

---

## ⚙️ 2. How tmpfs Works Internally in the Linux Kernel

When Docker initializes a `tmpfs` mount, it executes the Linux `mount -t tmpfs` system call inside the container's mount namespace.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   TMPFS IN-MEMORY ARCHITECTURE                         │
└────────────────────────────────────────────────────────────────────────┘

 Container View:
 /tmp/secrets/private_key.pem ──► Stored inside Container Target Path
         │
         │ (Redirected by Linux Kernel Mount Namespace)
         ▼
 Host Kernel Memory:
 [ Physical RAM / Page Cache ] ──► Resides purely in volatile memory chips.
                                   ❌ NEVER written to SSD/HDD storage!
                                   ⚡ Read/Write speeds at memory bus speeds (GB/s)!
                                   💥 Instantly vanished upon container shutdown!
```

### Key Behavioral Attributes:
1. **Linux-Exclusive Feature:** Because `tmpfs` relies on the Linux virtual filesystem kernel implementation, native `tmpfs` mounts are supported only on Linux hosts (or inside the Linux VM running Docker Desktop on macOS/Windows).
2. **Shared Memory (`/dev/shm`):** Docker automatically creates a default 64MB `tmpfs` mount at `/dev/shm` inside every container to support POSIX inter-process shared memory (IPC).
3. **Memory Limits and OOM Safety:** By default, if a `tmpfs` size limit is not configured, the mount can theoretically consume up to 50% of the host machine's total RAM. In production, operators must always enforce strict size boundaries (`tmpfs-size`) to prevent a runaway process from exhausting host memory and triggering the kernel Out-Of-Memory (OOM) killer.

---

## 🛡️ 3. Security Hardening Flags for tmpfs Mounts

Docker's `--mount type=tmpfs` syntax provides granular parameters to harden the in-memory mount point against privilege escalation and malicious script execution:

| Flag / Parameter | Purpose | Security Hardening Explanation |
| :--- | :--- | :--- |
| **`tmpfs-size=<bytes>`** | Cap maximum memory consumption | Prevents Denial of Service (DoS) attacks by strictly capping RAM usage (e.g. `67108864` for 64MB). |
| **`tmpfs-mode=<octal>`** | Set Unix permissions on mount root | Enforces least privilege directory permissions (e.g. `0700` for owner-only access). |
| **`noexec`** | Prohibit execution of binary files | Prevents attackers from downloading an exploit binary into `/tmp` and executing it. |
| **`nosuid`** | Ignore SUID and SGID bits | Blocks privilege escalation by preventing executables from running with elevated permissions. |
| **`nodev`** | Prohibit device node creation | Prevents processes from creating character or block device files via `mknod`. |

### Hardened Production Docker CLI Command:
```bash
docker run -d \
  --name security_vault \
  --mount type=tmpfs,target=/tmp/secrets,tmpfs-size=33554432,tmpfs-mode=0700 \
  mycompany/vault-service:v2.0
```

---

## 🏛️ 4. The Ultimate Hardening Standard: Read-Only Containers with tmpfs

The highest security benchmark defined by the **Center for Internet Security (CIS) Docker Benchmark** (Rule 5.12) is mounting the container's entire root filesystem as strictly **read-only (`--read-only`)**.

In this architecture, an attacker who gains access cannot modify application code, alter system binaries, or install malware. However, almost all software frameworks require write access to temporary directories (such as `/tmp`, `/run`, and `/var/run`) to function properly.

DevOps engineers solve this by pairing an immutable read-only rootfs with carefully bounded `tmpfs` scratch spaces:

```text
┌────────────────────────────────────────────────────────────────────────┐
│               IMMUTABLE CONTAINER + TMPFS SCRATCH SPACES               │
└────────────────────────────────────────────────────────────────────────┘

 Container Rootfs:
 [ / (Root Filesystem) ] ──────────► 🔒 READ-ONLY (Immutable! Cannot be modified)
 
 Scratch Directories:
 [ /tmp ]                ──────────► ✍️ tmpfs (64MB RAM, noexec, nosuid)
 [ /run ]                ──────────► ✍️ tmpfs (16MB RAM, noexec, nosuid)
 [ /var/run ]            ──────────► ✍️ tmpfs (16MB RAM, noexec, nosuid)
```

### Production Docker Compose Implementation:
```yaml
services:
  hardened-api:
    image: mycompany/production-api:v1.0.0
    restart: unless-stopped
    
    # 1. Enforce immutable root filesystem
    read_only: true
    
    # 2. Provide ephemeral in-memory write spaces for temporary files
    tmpfs:
      - /tmp:rw,noexec,nosuid,size=64m
      - /run:rw,noexec,nosuid,size=16m
      - /var/run:rw,noexec,nosuid,size=16m
      
    # 3. Drop all kernel privileges
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
      
    user: "10001:10001"
    
    networks:
      - internal_net

networks:
  internal_net:
    driver: bridge
```

---

## 📋 5. Summary and Navigation

Linux `tmpfs` mounts provide an indispensable tool for securing sensitive credentials and safeguarding ephemeral runtime memory. By storing sensitive secrets strictly in volatile RAM, capping memory boundaries, applying `noexec` security flags, and enforcing immutable read-only root filesystems, DevOps teams build hardened container architectures resilient against both forensic data recovery and runtime exploitation.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Volume Lifecycle Backup and Restoration](./03-Volume-Lifecycle-Backup-and-Restoration.md) | [Index](../../../README.md) | [05 - External Volume Plugins and Cloud Storage →](./05-External-Volume-Plugins-and-Cloud-Storage.md) |
