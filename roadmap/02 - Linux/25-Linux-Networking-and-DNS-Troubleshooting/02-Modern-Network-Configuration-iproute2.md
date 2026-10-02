# 02 — Modern Network Configuration (iproute2)

The legacy `net-tools` suite (`ifconfig`, `route`, `netstat`, `arp`) was declared obsolete in 2009. Modern Linux administration, automation, and container environments rely strictly on the `iproute2` suite, centered around the versatile `ip` command.

---

## 1. Why `iproute2` Replaced `net-tools`

| Feature | Legacy (`net-tools`) | Modern (`iproute2`) | Why `iproute2` Wins |
| :--- | :--- | :--- | :--- |
| **Kernel Interface** | Obsolete `ioctl()` calls | Linux Netlink Sockets | Netlink is asynchronous, bi-directional, and non-blocking. |
| **Address Handling** | Aliases required (`eth0:0`) | Secondary IPs natively on one device | Supports multiple IPs per interface without dummy aliases. |
| **Routing Tables** | Single routing table only | Multiple routing tables (Policy Routing) | Enables source-based routing, multi-NIC egress, and VPN routing. |
| **Network Namespaces**| Not supported | Fully supported (`ip netns`) | Essential for Docker, Podman, and Kubernetes CNI. |

---

## 2. Managing Link Layer (`ip link`)

`ip link` inspects and manages the physical or virtual network device state (Layer 2).

```bash
# List all network interfaces with MAC addresses and status
ip link show

# Display interface statistics (dropped packets, errors, collisions)
ip -s link show dev eth0

# Bring an interface up or down
sudo ip link set dev eth0 up
sudo ip link set dev eth0 down

# Change MTU (Maximum Transmission Unit)
sudo ip link set dev eth0 mtu 9000

# Change MAC address (must bring link down first)
sudo ip link set dev eth0 down
sudo ip link set dev eth0 address 00:11:22:33:44:55
sudo ip link set dev eth0 up

# Rename an interface
sudo ip link set dev eth1 name private0
```

---

## 3. Managing IP Addresses (`ip addr` / `ip a`)

`ip addr` manages IPv4 and IPv6 addresses attached to interfaces (Layer 3).

```bash
# Show all assigned IP addresses
ip addr show
ip -4 addr show   # IPv4 only
ip -6 addr show   # IPv6 only

# Add a static IP address with CIDR netmask
sudo ip addr add 192.168.1.150/24 dev eth0

# Add a secondary IP to the same interface (no alias needed!)
sudo ip addr add 10.0.0.50/24 dev eth0

# Remove an assigned IP address
sudo ip addr del 192.168.1.150/24 dev eth0

# Flush all IP addresses on an interface
sudo ip addr flush dev eth0
```

---

## 4. Managing Routing (`ip route` / `ip r`)

`ip route` displays and manipulates the kernel IP routing table.

```bash
# Display the active routing table
ip route show

# Add a default gateway (all non-local traffic exits via 192.168.1.1)
sudo ip route add default via 192.168.1.1 dev eth0

# Add a static route for a private subnet via a specific gateway
sudo ip route add 10.100.0.0/16 via 192.168.1.254 dev eth0

# Add a blackhole route (drops packets immediately with zero network overhead)
sudo ip route add blackhole 198.51.100.0/24

# Delete a route
sudo ip route del 10.100.0.0/16

# Determine which route and interface the kernel will select for a destination
ip route get 8.8.8.8
# Example output:
# 8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.150 uid 1000 \ cache
```

---

## 5. Neighbor Discovery & ARP (`ip neigh`)

Replaces the legacy `arp` command to manage Layer 2 to Layer 3 mappings.

```bash
# Show ARP cache (IP to MAC mapping)
ip neigh show

# Flush the ARP cache (forces re-discovery of gateways)
sudo ip neigh flush all

# Manually insert a static ARP entry
sudo ip neigh add 192.168.1.1 lladdr 00:aa:bb:cc:dd:ee dev eth0 nud permanent
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Linux Network Stack](./01-Linux-Network-Stack-and-Device-Model.md) | [README](./README.md) | [03 - Socket and Port Inspection](./03-Socket-and-Port-Inspection-ss-and-netstat.md) |
