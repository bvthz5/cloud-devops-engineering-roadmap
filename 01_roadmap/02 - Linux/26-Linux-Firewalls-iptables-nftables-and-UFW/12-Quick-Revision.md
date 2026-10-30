# 12 — Quick Revision Cheat Sheet: Linux Firewalls

A high-density reference comparing firewall frameworks across Linux distributions.

---

## 1. Quick Command Comparison

| Task | `iptables` | `nftables` | `UFW` | `firewalld` |
| :--- | :--- | :--- | :--- | :--- |
| **Check status / list** | `iptables -L -n -v` | `nft list ruleset` | `ufw status numbered` | `firewall-cmd --list-all` |
| **Allow port 80/tcp** | `iptables -A INPUT -p tcp --dport 80 -j ACCEPT` | `nft add rule inet filter input tcp dport 80 accept` | `ufw allow 80/tcp` | `firewall-cmd --permanent --add-port=80/tcp` |
| **Default drop policy**| `iptables -P INPUT DROP`| `chain input { policy drop; }`| `ufw default deny incoming` | `firewall-cmd --set-default-zone=drop` |
| **Save / Persist** | `netfilter-persistent save` | `nft list ruleset > /etc/nftables.conf` | *(Automatic)* | `firewall-cmd --runtime-to-permanent` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (27-systemd-Service-Management-and-Journald) →](../27-systemd-Service-Management-and-Journald/01-systemd-Architecture-and-PID-1.md) |
