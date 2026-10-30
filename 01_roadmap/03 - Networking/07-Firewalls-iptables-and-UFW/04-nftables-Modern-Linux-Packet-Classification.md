# 04 - nftables: Modern Linux Packet Classification

## 1. Why nftables Replaces iptables

`nftables` is the modern successor to `iptables`, `ip6tables`, `arptables`, and `ebtables`.
Key architectural improvements:
- **Unified Engine:** Single user-space utility (`nft`) replaces four distinct tools.
- **In-Kernel Virtual Machine:** Rules are compiled into bytecode and executed inside a lightweight kernel VM, drastically improving evaluation speed.
- **Atomic Rule Updates:** Entire rule sets can be replaced or modified atomically without race conditions or connection interruptions.
- **Sets and Dictionaries:** Native support for sets, IP maps, and verdict maps eliminates the need for massive rule chains.

---

## 2. Syntax Comparison: iptables vs. nftables

| Operation | `iptables` | `nftables` |
|---|---|---|
| Open Port 80 & 443 | `iptables -A INPUT -p tcp -m multiport --dports 80,443 -j ACCEPT` | `nft add rule inet filter input tcp dport { 80, 443 } accept` |
| Block IP Subnet | `iptables -A INPUT -s 192.168.100.0/24 -j DROP` | `nft add rule inet filter input ip saddr 192.168.100.0/24 drop` |
| NAT Port Forward | `iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080` | `nft add rule inet nat prerouting tcp dport 80 redirect to :8080` |

---

## 3. Configuring nftables

```nftables
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
    chain input {
        type filter hook input priority 0; policy drop;

        # Accept loopback
        iif "lo" accept

        # Accept established and related traffic
        ct state { established, related } accept
        ct state invalid drop

        # Accept SSH, HTTP, HTTPS
        tcp dport { 22, 80, 443 } accept

        # Accept ICMP ping
        ip protocol icmp accept
        ip6 nexthdr icmpv6 accept
    }

    chain forward {
        type filter hook forward priority 0; policy drop;
    }

    chain output {
        type filter hook output priority 0; policy accept;
    }
}
```

```bash
# Check syntax and load ruleset
nft -c -f /etc/nftables.conf
nft -f /etc/nftables.conf

# List active ruleset
nft list ruleset
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Stateful Firewall Inspection and Conntrack](./03-Stateful-Firewall-Inspection-and-Conntrack.md) | [Index](../../../README.md) | [05 - Host Firewalls UFW and firewalld Management →](./05-Host-Firewalls-UFW-and-firewalld-Management.md) |
