# 03 — Hardening and Sandboxing Services with systemd

One of systemd's most powerful yet underutilized features is its ability to turn any unprivileged process into a secure, sandboxed container-like environment using Linux kernel namespaces, seccomp filters, and capabilities.

---

## 1. Auditing Security with `systemd-analyze security`

systemd includes a built-in security auditing engine that scores any running service from 0.0 (Extremely Secure) to 10.0 (Dangerous / Exposed):

```bash
systemd-analyze security api-gateway.service
```

---

## 2. Production Sandboxing Directives

Add these directives to the `[Service]` block of your unit file:

```ini
[Service]
# Run as non-root user
User=appuser
Group=appuser

# 1. Filesystem Isolation
# Mounts /usr, /boot, /etc as read-only for this process
ProtectSystem=strict

# Ensures process cannot read or access /home, /root, or /run/user
ProtectHome=true

# Dedicated isolated /tmp directory per service (prevent tmp race conditions)
PrivateTmp=true

# Allow writes ONLY to specific designated application directories
ReadWritePaths=/var/log/api-gateway /var/lib/api-gateway

# 2. Kernel & Hardware Protection
# Prevent process or its children from gaining new privileges (setuid/setgid)
NoNewPrivileges=true

# Restrict access to raw hardware devices (/dev)
PrivateDevices=true

# Deny access to kernel tunables (/proc/sys, /sys)
ProtectKernelTunables=true
ProtectKernelModules=true
ProtectControlGroups=true

# 3. System Call Filtering (Seccomp)
# Deny dangerous system calls (reboot, clock changes, obsolete syscalls)
ProtectClock=true
ProtectHostname=true
SystemCallFilter=@system-service
SystemCallFilter=~@privileged @resources

# 4. Capability Dropping
# Strip all Linux kernel capabilities except binding to unprivileged ports
CapabilityBoundingSet=
AmbientCapabilities=
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Writing Custom systemd Service Units](./02-Writing-Custom-systemd-Service-Units.md) | [Index](../../../README.md) | [04 - systemd Timers The Modern Cron Replacement →](./04-systemd-Timers-The-Modern-Cron-Replacement.md) |
