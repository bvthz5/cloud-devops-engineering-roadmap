# 10 - Hands-On Practice Labs

## Lab 1: Manually Creating a Network Namespace with `ip netns`

### Objective
Create two isolated Linux network namespaces, connect them via a virtual ethernet pair (`veth`), and verify ping connectivity across the isolated boundary.

### Implementation
```bash
# 1. Create two network namespaces
sudo ip netns add red
sudo ip netns add blue

# 2. Create virtual ethernet cable (veth pair)
sudo ip link add veth-red type veth peer name veth-blue

# 3. Attach ends to respective namespaces
sudo ip link set veth-red netns red
sudo ip link set veth-blue netns blue

# 4. Configure IP addresses and bring up interfaces
sudo ip netns exec red ip addr add 192.168.1.1/24 dev veth-red
sudo ip netns exec red ip link set veth-red up
sudo ip netns exec red ip link set lo up

sudo ip netns exec blue ip addr add 192.168.1.2/24 dev veth-blue
sudo ip netns exec blue ip link set veth-blue up
sudo ip netns exec blue ip link set lo up

# 5. Ping from red to blue across isolated namespace!
sudo ip netns exec red ping -c 3 192.168.1.2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
