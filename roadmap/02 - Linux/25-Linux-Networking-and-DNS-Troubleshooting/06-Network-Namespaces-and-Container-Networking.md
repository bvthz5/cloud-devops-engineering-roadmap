# 06 — Network Namespaces and Container Networking

Every container running in Docker, Podman, or Kubernetes relies on Linux **Network Namespaces** (`netns`) to achieve network isolation. Understanding namespaces demystifies how containers get their own IP addresses, routing tables, and firewall rules.

---

## 1. What is a Network Namespace?

A Network Namespace provides a private, isolated instance of the Linux network stack, including:
- Its own network interfaces (`lo`, `eth0`)
- Its own IP routing tables
- Its own ARP / Neighbor cache
- Its own Netfilter / iptables rules
- Its own socket list (`ss -tulpn`)

The initial root namespace (`init_net`) is created when Linux boots. All standard host processes run inside this root namespace.

---

## 2. The Container Networking Anatomy

How does a container talk to another container or the external internet?

```text
+-----------------------+              +-----------------------+
|  Container A (netns1) |              |  Container B (netns2) |
|  IP: 172.17.0.2       |              |  IP: 172.17.0.3       |
|  eth0 (vethA_peer)    |              |  eth0 (vethB_peer)    |
+-----------|-----------+              +-----------|-----------+
            | (veth pair)                          | (veth pair)
            v                                      v
+-----------|--------------------------------------|-----------+
| Host Root Namespace (init_net)                               |
|                                                              |
|        vethA                                  vethB          |
|          \                                      /            |
|           +----------> [ docker0 / cbr0 ] <-----+            |
|                           (Linux Bridge)                     |
|                                  |                           |
|                             NAT / MASQUERADE                 |
|                                  v                           |
|                             eth0 (Physical)                  |
|                                  |                           |
+----------------------------------|---------------------------+
                                   v
                             Internet / VPC
```

1. **Virtual Ethernet (`veth`) Pair:** Acts like a virtual ethernet cable. Packets pushed into one end (`vethA`) emerge out the other end (`vethA_peer`).
2. **Namespace Assignment:** One end of the cable stays in the host root namespace, while the other end is moved into the container's private namespace and renamed to `eth0`.
3. **Bridge Device (`docker0`):** Acts as a software switch in the host namespace, bridging all container `veth` ends together.
4. **IP Masquerading (NAT):** Netfilter rules rewrite the container's private IP (`172.17.0.2`) to the host's public/VPC IP when exiting `eth0`.

---

## 3. Working Directly with `ip netns`

You can manually build this entire container networking architecture using standard Linux commands:

```bash
# 1. Create two isolated network namespaces
sudo ip netns add red
sudo ip netns add blue

# 2. List active network namespaces
ip netns list

# 3. Create a virtual ethernet (veth) cable
sudo ip link add veth-red type veth peer name veth-blue

# 4. Attach each end to its respective namespace
sudo ip link set veth-red netns red
sudo ip link set veth-blue netns blue

# 5. Configure IP addresses inside the namespaces
sudo ip netns exec red ip addr add 10.0.0.1/24 dev veth-red
sudo ip netns exec red ip link set dev veth-red up
sudo ip netns exec red ip link set dev lo up

sudo ip netns exec blue ip addr add 10.0.0.2/24 dev veth-blue
sudo ip netns exec blue ip link set dev veth-blue up
sudo ip netns exec blue ip link set dev lo up

# 6. Test ping between the two isolated environments!
sudo ip netns exec red ping -c 3 10.0.0.2
```

---

## 4. Kubernetes CNI (Container Network Interface)

Kubernetes does not implement networking natively; it delegates networking to a CNI plugin (e.g., Calico, Cilium, AWS VPC CNI, Flannel).
- **Flannel:** Creates VXLAN tunnels to encapsulate pod packets over standard host IP networks.
- **Calico:** Uses BGP (Border Gateway Protocol) to turn each Kubernetes node into a router, advertising pod subnets natively.
- **Cilium:** Uses Linux eBPF inside the kernel to route packets directly between container sockets without iptables overhead.
- **AWS VPC CNI:** Attaches real AWS Elastic Network Interfaces (ENIs) directly to pods, giving each pod a real VPC IP address.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Packet Analysis and Diagnostics](./05-Packet-Analysis-and-Diagnostics-tcpdump-traceroute-ping.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
