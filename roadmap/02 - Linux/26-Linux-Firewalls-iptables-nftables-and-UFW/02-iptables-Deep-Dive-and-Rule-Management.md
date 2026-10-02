# 02 — iptables Deep Dive and Rule Management

`iptables` is the traditional user-space utility used to configure IPv4 packet filtering rules in the Netfilter kernel framework (`ip6tables` handles IPv6).

---

## 1. The Tables and Chains Hierarchy

`iptables` organizes rules into **Tables** based on function, and each table contains **Chains** corresponding to Netfilter hooks:

| Table | Primary Purpose | Built-in Chains |
| :--- | :--- | :--- |
| **`filter`** (Default) | Packet filtering (accept or drop) | `INPUT`, `FORWARD`, `OUTPUT` |
| **`nat`** | Network Address Translation (IP / Port rewrites)| `PREROUTING`, `INPUT`, `OUTPUT`, `POSTROUTING` |
| **`mangle`** | Modifying packet headers (TOS, TTL, Mark) | `PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING` |
| **`raw`** | Configure exemptions from connection tracking | `PREROUTING`, `OUTPUT` |

---

## 2. Common Rule Targets (`-j`)

When a rule matches a packet, it executes a target action via `-j` (Jump):
- **`ACCEPT`:** Allows the packet to proceed through the network stack.
- **`DROP`:** Silently discards the packet with zero response to the sender (stealth mode).
- **`REJECT`:** Discards the packet and sends an error back (e.g. `TCP RST` for TCP, or `ICMP Port Unreachable` for UDP).
- **`LOG`:** Logs packet headers to syslog / journald without terminating rule traversal.
- **`MASQUERADE`:** Dynamic Source NAT (uses the egress interface's current IP).
- **`DNAT`:** Destination NAT (redirects incoming packets to another IP/port).

---

## 3. Essential `iptables` Commands

```bash
# List all rules in the default 'filter' table with line numbers and packet counters
sudo iptables -L -n -v --line-numbers

# List rules in the 'nat' table
sudo iptables -t nat -L -n -v

# Set default policy for chains (DROP by default = Zero Trust)
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Allow loopback interface (MANDATORY for internal processes)
sudo iptables -A INPUT -i lo -j ACCEPT

# Stateful filtering: Allow established and related return traffic
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow incoming SSH (port 22)
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT

# Allow incoming HTTP/HTTPS from a specific subnet only
sudo iptables -A INPUT -p tcp -s 10.0.0.0/16 -m multiport --dports 80,443 -j ACCEPT

# Delete a rule by its line number
sudo iptables -D INPUT 3

# Flush (delete all) rules in filter table
sudo iptables -F
```

---

## 4. Making iptables Rules Persistent

By default, `iptables` rules are stored in kernel memory and vanish upon reboot.
- **Debian / Ubuntu:**
  ```bash
  sudo apt-get install -y iptables-persistent
  # Save current rules to disk:
  sudo netfilter-persistent save
  ```
- **RHEL / Rocky Linux:**
  ```bash
  sudo dnf install -y iptables-services
  sudo systemctl enable --now iptables
  sudo iptables-save > /etc/sysconfig/iptables
  ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Netfilter Architecture](./01-Linux-Packet-Filtering-and-Netfilter-Architecture.md) | [README](./README.md) | [03 - nftables Modern Firewall](./03-nftables-The-Modern-Linux-Firewall.md) |
