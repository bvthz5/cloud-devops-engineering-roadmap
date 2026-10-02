# 08 — OSI Layer-by-Layer Troubleshooting Guide

Follow this bottom-up troubleshooting methodology during any network incident:

---

## The Bottom-Up Checklist

```text
Layer 1 (Physical):
  - Is the link up? `ip link show` (Look for <UP,LOWER_UP>)
  - Cable / SFP transceiver errors? `ethtool eth0`

Layer 2 (Data Link):
  - Did the gateway MAC resolve? `ip neigh show`
  - Are frames dropping? `ip -s link show dev eth0`

Layer 3 (Network):
  - Do we have an IP address? `ip addr show`
  - Is the default route present? `ip route show`
  - Can we ping the gateway? `ping <gateway_ip>`
  - Where is the path breaking? `traceroute -n <target_ip>`

Layer 4 (Transport):
  - Is the port open and listening? `ss -tulpn | grep <port>`
  - Is a firewall dropping SYN packets? `sudo iptables -S`

Layer 7 (Application):
  - Does DNS resolve? `dig +short <hostname>`
  - Does HTTP respond? `curl -Iv https://<hostname>`
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
