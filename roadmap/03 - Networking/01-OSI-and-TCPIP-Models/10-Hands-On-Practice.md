# 10 — Hands-On Practice Labs: OSI & TCP/IP

---

## Lab 1: Inspecting Packet Encapsulation with tcpdump

```bash
# Capture 2 ICMP packets and print full link-layer Ethernet headers
sudo tcpdump -i any -c 2 -e -nn icmp &

# Trigger ping
ping -c 1 8.8.8.8

# Observe output:
# Shows Source MAC, Destination MAC, EtherType (IPv4), Source IP, Destination IP, and ICMP type!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
