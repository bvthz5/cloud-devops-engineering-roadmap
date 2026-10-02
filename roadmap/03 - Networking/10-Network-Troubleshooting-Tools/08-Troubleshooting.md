# 08 - Troubleshooting Tools: SRE Rapid Network Triage

## SRE 7-Step Network Outage Runbook

```
Step 1: Check Physical / Link Layer
        ip link show (Are interfaces UP? Are RX/TX errors climbing?)
          │
Step 2: Check Network Routing
        ip route get <destination> (Does a valid route exist? What is the next hop?)
          │
Step 3: Test Layer 3 Reachability
        ping -c 3 <destination> (Is the host reachable? What is the baseline RTT?)
          │
Step 4: Check DNS Resolution
        dig +trace <hostname> (Does domain resolve? Are TTLs expired?)
          │
Step 5: Test Layer 4 Port Reachability
        nc -zv -w 2 <ip> <port> (Is the port open, closed, or filtered?)
          │
Step 6: Inspect Listening Sockets on Target Host
        ss -tulpn | grep <port> (Is the service listening on 0.0.0.0 or 127.0.0.1?)
          │
Step 7: Capture Raw Packets
        tcpdump -nn -i any port <port> (Are SYNs arriving? Are RSTs being sent?)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
