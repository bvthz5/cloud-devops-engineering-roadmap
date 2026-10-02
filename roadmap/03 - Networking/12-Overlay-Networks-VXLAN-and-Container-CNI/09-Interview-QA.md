# 09 - Overlay Networks & CNI: Interview Questions & Answers

### Q1: Why does Cilium offer significantly higher throughput and lower CPU utilization than standard iptables-based kube-proxy?
**Answer:** `kube-proxy` relies on sequential traversal of thousands of Netfilter iptables chains ($O(n)$ complexity). Cilium uses Linux kernel **eBPF (Extended Berkeley Packet Filter)** maps ($O(1)$ hash table lookups) and bypasses the entire kernel TCP/IP stack overhead using socket-level redirection (sockops).

### Q2: What causes a connection over a VXLAN overlay to hang during TLS handshakes while ping works?
**Answer:** An **MTU mismatch**. Ping uses tiny ICMP packets (64 bytes) that fit comfortably within the MTU. TLS Client Hello or Certificate exchanges transmit packets close to 1500 bytes. With 50 bytes of VXLAN header added, packets exceed physical MTU 1500 and are silently dropped.

### Q3: What are the three primary actions executed by a CNI plugin binary?
**Answer:** The CNI specification defines:
1. `ADD`: Attach a container to the network (create veth, assign IP, configure routes).
2. `DEL`: Tear down the network interface and release the IP address back to the pool.
3. `CHECK`: Validate that the container's networking configuration matches expectations.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
