# 10 - Hands-On Practice: Setting Up an eBGP Session with FRRouting (FRR)

## Lab Scenario
Establish an eBGP session on Linux using FRRouting between Router 1 (`ASN 65001`, IP `10.0.0.1`) and Router 2 (`ASN 65002`, IP `10.0.0.2`).

---

## Lab Steps

### Step 1: Install FRR on Linux
```bash
sudo apt update && sudo apt install -y frr
sudo sed -i 's/bgpd=no/bgpd=yes/' /etc/frr/daemons
sudo systemctl restart frr
```

### Step 2: Configure BGP on Router 1 (`vtysh`)
```bash
sudo vtysh
conf t
router bgp 65001
  neighbor 10.0.0.2 remote-as 65002
  neighbor 10.0.0.2 description PEER_ROUTER_2
  address-family ipv4 unicast
    network 192.168.10.0/24
    neighbor 10.0.0.2 activate
  exit-address-family
exit
exit
write memory
```

### Step 3: Verify BGP Session Status
```bash
show ip bgp summary
show ip bgp neighbors 10.0.0.2 routes
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
