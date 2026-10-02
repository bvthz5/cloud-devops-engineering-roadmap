# 10 - Hands-On Practice: Building Container Networking From Scratch

## Lab Scenario
Manually construct two isolated Linux network namespaces, connect them using a `veth` pair and a virtual Linux bridge, and achieve cross-namespace communication without Docker.

---

## Lab Steps

### Step 1: Create Two Network Namespaces
```bash
sudo ip netns add ns-red
sudo ip netns add ns-blue
```

### Step 2: Create a Linux Bridge
```bash
sudo ip link add br0 type bridge
sudo ip link set br0 up
sudo ip addr add 10.200.0.1/24 dev br0
```

### Step 3: Create Veth Pairs and Connect to Bridge
```bash
# Red namespace
sudo ip link add veth-red type veth peer name veth-red-br
sudo ip link set veth-red netns ns-red
sudo ip link set veth-red-br master br0
sudo ip link set veth-red-br up

# Blue namespace
sudo ip link add veth-blue type veth peer name veth-blue-br
sudo ip link set veth-blue netns ns-blue
sudo ip link set veth-blue-br master br0
sudo ip link set veth-blue-br up
```

### Step 4: Configure IPs and Default Routes
```bash
sudo ip netns exec ns-red ip addr add 10.200.0.10/24 dev veth-red
sudo ip netns exec ns-red ip link set veth-red up
sudo ip netns exec ns-red ip route add default via 10.200.0.1

sudo ip netns exec ns-blue ip addr add 10.200.0.20/24 dev veth-blue
sudo ip netns exec ns-blue ip link set veth-blue up
sudo ip netns exec ns-blue ip route add default via 10.200.0.1
```

### Step 5: Verify Connectivity
```bash
sudo ip netns exec ns-red ping 10.200.0.20
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
