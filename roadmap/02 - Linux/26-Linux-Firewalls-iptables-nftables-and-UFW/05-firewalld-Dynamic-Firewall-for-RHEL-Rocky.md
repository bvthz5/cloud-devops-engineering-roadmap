# 05 — firewalld: Dynamic Firewall Management for RHEL & Rocky Linux

`firewalld` is the default firewall management solution on Red Hat Enterprise Linux (RHEL), CentOS Stream, Rocky Linux, AlmaLinux, and Fedora. Unlike static firewalls that require flushing and re-reading all rules upon changes, `firewalld` applies changes dynamically via D-Bus without dropping existing network connections.

---

## 1. Core Architecture: Zones

`firewalld` organizes rules into **Zones**, representing the level of trust you assign to network interfaces:

| Zone | Trust Level | Typical Use Case |
| :--- | :--- | :--- |
| **`drop`** | Lowest | All incoming packets are dropped without response. |
| **`block`** | Low | All incoming packets are rejected with ICMP error. |
| **`public`** (Default)| Untrusted | Only explicitly allowed incoming ports are accepted. |
| **`internal`** | Moderate | For internal VPC or private subnet traffic. |
| **`trusted`** | Maximum | All network connections are accepted. |

---

## 2. Managing firewalld with `firewall-cmd`

> [!IMPORTANT]
> `firewall-cmd` operates in two modes:
> - **Runtime (Default):** Applies changes immediately, but they are lost upon reload or reboot.
> - **Permanent (`--permanent`):** Persists across reboots, but requires `--reload` to become active.

```bash
# Check status and default zone
sudo firewall-cmd --state
sudo firewall-cmd --get-default-zone

# List all rules in the active zone
sudo firewall-cmd --list-all

# Allow a service permanently
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https

# Allow a custom port
sudo firewall-cmd --permanent --add-port=8080/tcp

# Restrict port access to a specific source subnet using Rich Rules
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.0.1.0/24" port port="6379" protocol="tcp" accept'

# Reload firewalld to activate permanent rules into runtime
sudo firewall-cmd --reload
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - UFW for Debian & Ubuntu](./04-UFW-Uncomplicated-Firewall-for-Debian-Ubuntu.md) | [README](./README.md) | [06 - NAT, Port Forwarding & Masquerading](./06-NAT-Port-Forwarding-and-Masquerading.md) |
