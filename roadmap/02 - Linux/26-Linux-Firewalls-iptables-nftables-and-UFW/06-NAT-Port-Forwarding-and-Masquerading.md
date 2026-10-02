# 06 — NAT, Port Forwarding, and Masquerading

Network Address Translation (NAT) modifies the source or destination IP addresses and ports of packets in flight. It enables private networks (e.g. AWS private subnets, Docker bridges) to access the internet via a single public IP, and routes external traffic to internal container endpoints.

---

## 1. SNAT vs DNAT vs MASQUERADE

```text
[ SNAT: Source NAT ]
Client (10.0.0.5) -----> [ Gateway Router ] -----> Internet (Target: 8.8.8.8)
                         Rewrites Source IP:
                         10.0.0.5 -> 203.0.113.1

[ DNAT: Destination NAT / Port Forwarding ]
Internet Client -------> [ Gateway (203.0.113.1:8080) ] -----> Backend Pod (172.17.0.2:80)
                         Rewrites Dest IP:Port:
                         203.0.113.1:8080 -> 172.17.0.2:80
```

- **SNAT (Source NAT):** Modifies the source IP address in the `POSTROUTING` chain. Used when private hosts initiate outbound connections.
- **MASQUERADE:** A specialized form of SNAT designed for dynamic IP interfaces (DHCP/PPPoE). Checks the interface IP dynamically per packet.
- **DNAT (Destination NAT):** Modifies the destination IP address/port in the `PREROUTING` chain. Used for port forwarding.

---

## 2. Configuring NAT Router with `iptables`

Prerequisite: Enable kernel IPv4 forwarding:
```bash
sudo sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/99-ipforward.conf
```

### 2.1 Enable Outbound Masquerading (SNAT)
Allow private subnet `10.0.0.0/24` on `eth1` to access the internet via public interface `eth0`:
```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -s 10.0.0.0/24 -j MASQUERADE
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o eth1 -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

### 2.2 Configure Port Forwarding (DNAT)
Forward incoming public traffic on port `8080` to an internal backend server `10.0.0.25:80`:
```bash
# 1. Rewrite destination IP and port
sudo iptables -t nat -A PREROUTING -p tcp -d 203.0.113.1 --dport 8080 -j DNAT --to-destination 10.0.0.25:80

# 2. Allow forwarding through the filter table
sudo iptables -A FORWARD -p tcp -d 10.0.0.25 --dport 80 -m conntrack --ctstate NEW,ESTABLISHED,RELATED -j ACCEPT
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - firewalld for RHEL & Rocky](./05-firewalld-Dynamic-Firewall-for-RHEL-Rocky.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
