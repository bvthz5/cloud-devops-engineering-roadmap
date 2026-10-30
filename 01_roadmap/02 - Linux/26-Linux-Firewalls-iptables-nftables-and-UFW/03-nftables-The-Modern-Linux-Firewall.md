# 03 — nftables: The Modern Linux Firewall

`nftables` is the official modern replacement for `iptables`, `ip6tables`, `arptables`, and `ebtables`. Introduced in Linux kernel 3.13, it unifies all packet filtering into a single bytecode-driven virtual machine framework.

---

## 1. Why `nftables` Replaced `iptables`

| Feature | Legacy `iptables` | Modern `nftables` |
| :--- | :--- | :--- |
| **Protocol Support** | Separate tools (`iptables`, `ip6tables`, `ebtables`)| Single unified tool (`nft`) for IPv4, IPv6, ARP, Bridge |
| **Rule Updates** | Non-atomic: full table re-read and write | **Atomic** transactions: update rules without race conditions |
| **Performance** | O(N) linear search through sequential rules | **O(1)** lookup using native Sets, Dictionaries, and Concatenations |
| **Syntax** | Rigid, flag-heavy CLI (`-A`, `-p`, `-m`, `--dport`) | Human-readable, structured configuration language |
| **Code Base** | Duplicated across 4 separate kernel modules | Single clean kernel bytecode engine (similar to BPF) |

---

## 2. `nftables` Architecture: Tables, Chains, and Sets

Unlike `iptables`, `nftables` has **no predefined tables or chains**. You create only what you need.

```text
Table (e.g. inet filter)
   ├── Chain: input [type filter hook input priority 0; policy drop;]
   │      ├── Rule: ct state established,related accept
   │      ├── Rule: iif "lo" accept
   │      ├── Rule: tcp dport 22 accept
   │      └── Rule: ip saddr @trusted_ips tcp dport 80 accept
   └── Set: @trusted_ips { 192.168.1.50, 10.0.0.0/24 }
```

---

## 3. High-Performance nftables Rule Example

Configuration file: `/etc/nftables.conf`

```nft
#!/usr/sbin/nft -f

# Flush existing ruleset
flush ruleset

# Create unified IPv4/IPv6 table
table inet my_firewall {

    # Define a high-speed Set of allowed admin IPs
    set admin_ips {
        type ipv4_addr
        flags interval
        elements = { 10.0.1.0/24, 192.168.1.100 }
    }

    chain incoming {
        type filter hook input priority 0; policy drop;

        # Accept loopback traffic
        iif "lo" accept

        # Accept established/related connections
        ct state established,related accept

        # Drop invalid packets
        ct state invalid drop

        # Allow SSH only from admin IPs
        ip saddr @admin_ips tcp dport 22 accept

        # Allow public Web traffic (HTTP & HTTPS in a single rule!)
        tcp dport { 80, 443 } accept

        # Allow ping with rate limiting
        ip protocol icmp icmp type echo-request limit rate 5/second accept
    }
}
```

Apply and inspect:
```bash
# Check syntax and load file atomically
sudo nft -f /etc/nftables.conf

# List active ruleset
sudo nft list ruleset
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - iptables Deep Dive and Rule Management](./02-iptables-Deep-Dive-and-Rule-Management.md) | [Index](../../../README.md) | [04 - UFW Uncomplicated Firewall for Debian Ubuntu →](./04-UFW-Uncomplicated-Firewall-for-Debian-Ubuntu.md) |
