# 10 — Hands-On Practice Labs: Linux Networking & DNS

Complete these practical labs to reinforce Linux networking concepts.

---

## Lab 1: Manually Creating Container Networking with `ip netns`

### Objective
Create two isolated network namespaces and connect them using a virtual ethernet (`veth`) pair and a software bridge, replicating what Docker does under the hood.

### Commands:
```bash
# 1. Create bridge interface on host
sudo ip link add name br0 type bridge
sudo ip addr add 172.20.0.1/24 dev br0
sudo ip link set dev br0 up

# 2. Create isolated container namespace
sudo ip netns add container1

# 3. Create veth pair
sudo ip link add veth-c1 type veth peer name veth-c1-peer

# 4. Attach one end to bridge on host
sudo ip link set veth-c1 master br0
sudo ip link set veth-c1 up

# 5. Move other end into container namespace
sudo ip link set veth-c1-peer netns container1

# 6. Configure IP and default route inside container namespace
sudo ip netns exec container1 ip link set dev lo up
sudo ip netns exec container1 ip link set dev veth-c1-peer name eth0
sudo ip netns exec container1 ip addr add 172.20.0.10/24 dev eth0
sudo ip netns exec container1 ip link set dev eth0 up
sudo ip netns exec container1 ip route add default via 172.20.0.1

# 7. Test bidirectional communication!
ping -c 3 172.20.0.10
sudo ip netns exec container1 ping -c 3 172.20.0.1
```

---

## Lab 2: Capturing and Inspecting Live HTTP/DNS Packets with `tcpdump`

### Objective
Capture live DNS queries and HTTP payloads, filter using BPF syntax, and inspect request headers.

### Commands:
```bash
# 1. Open a terminal and start packet capture for DNS (port 53)
sudo tcpdump -i any -nn -v 'port 53' -c 4

# 2. In another terminal, trigger a DNS lookup
dig example.com

# 3. Capture HTTP traffic and print ASCII payload
sudo tcpdump -i any -nn -A -s 0 'tcp port 80' -c 10
# In another terminal:
curl -s http://example.com > /dev/null
```

---

## Lab 3: Deep DNS Resolution Tracing with `dig`

### Objective
Trace a domain's resolution hierarchically from the 13 Root DNS servers down to the authoritative nameserver.

### Commands:
```bash
# Trace full delegation path
dig +trace www.kernel.org

# Observe the 4 distinct resolution tiers in the output:
# Tier 1: Root (.) zone servers (a.root-servers.net to m.root-servers.net)
# Tier 2: Top-Level Domain (TLD) .org servers
# Tier 3: Authoritative nameservers for kernel.org
# Tier 4: Final 'A' record answer
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
