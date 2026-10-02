# 02 - iptables Tables, Chains, and Rule Syntax

## 1. iptables Architecture: Tables and Chains

`iptables` organizes rules into **Tables** based on functional purpose. Each table contains built-in **Chains** corresponding to the Netfilter hooks:

| Table | Purpose | Supported Built-in Chains |
|---|---|---|
| `raw` | High-performance packet exemption from connection tracking (`NOTRACK`). Evaluated first. | `PREROUTING`, `OUTPUT` |
| `mangle` | Modifying packet IP headers (TOS, TTL, MARK for policy routing). | `PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING` |
| `nat` | Network Address Translation (translating source or destination IPs/ports). | `PREROUTING`, `INPUT`, `OUTPUT`, `POSTROUTING` |
| `filter` | Default firewall filtering (deciding whether a packet is accepted or dropped). | `INPUT`, `FORWARD`, `OUTPUT` |
| `security` | Mandatory Access Control (MAC) networking rules (used by SELinux / AppArmor). | `INPUT`, `FORWARD`, `OUTPUT` |

---

## 2. Table Evaluation Order

When a packet hits a hook, tables are evaluated in this strict order:
1. `raw`
2. `mangle`
3. `nat` (only for the first packet of a connection)
4. `filter`
5. `security`

---

## 3. Core Rule Syntax and Targets

An `iptables` rule consists of criteria (matches) and a target (action):

```bash
iptables [-t table] [ACTION] [CHAIN] [CRITERIA] -j [TARGET]
```

### Common Targets (`-j`)
- `ACCEPT`: Allow the packet through. Stop evaluating further rules in this chain.
- `DROP`: Silently discard the packet without informing the sender (client hangs until timeout).
- `REJECT`: Discard the packet and send an ICMP error packet (`port-unreachable` or `tcp-reset`) back to the sender.
- `MASQUERADE`: Specialized SNAT for dynamically assigned IP addresses (DHCP/public IPs).
- `SNAT`: Rewrite source IP to a static IP (`--to-source <IP>`).
- `DNAT`: Rewrite destination IP and/or port (`--to-destination <IP>:<PORT>`).
- `LOG`: Log packet metadata to kernel syslog (`dmesg` / `/var/log/kern.log`) and continue evaluating subsequent rules.

---

## 4. Practical iptables Commands

```bash
# List all rules in the default (filter) table with packet counts and line numbers
iptables -vnL --line-numbers

# List rules in the NAT table
iptables -t nat -vnL --line-numbers

# Set default policy for INPUT chain to DROP
iptables -P INPUT DROP

# Allow loopback interface (MANDATORY for local system services)
iptables -A INPUT -i lo -j ACCEPT

# Allow already established and related connections
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow inbound SSH on port 22 from specific management subnet
iptables -A INPUT -p tcp -s 10.0.50.0/24 --dport 22 -j ACCEPT

# Allow inbound HTTP/HTTPS from anywhere
iptables -A INPUT -p tcp -m multiport --dports 80,443 -j ACCEPT

# Delete a rule by line number
iptables -D INPUT 3
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Netfilter Architecture](./01-Linux-Netfilter-Architecture-and-Hooks.md) | [README](./README.md) | [03 - Stateful Inspection](./03-Stateful-Firewall-Inspection-and-Conntrack.md) |
