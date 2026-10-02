# 04 — DHCP Protocol and the DORA Process

Dynamic Host Configuration Protocol (DHCP) automatically configures network settings (IP address, netmask, default gateway, DNS servers) for hosts connecting to a network.

---

## 1. The 4-Step DORA Handshake

Operates over **UDP Port 67 (Server)** and **UDP Port 68 (Client)**:

```text
Client (0.0.0.0:68)                                   DHCP Server (67)
       │                                                     │
       │ ─── 1. DISCOVER (Broadcast: 255.255.255.255) ───────>│ "Is there any DHCP server?"
       │                                                     │
       │ <── 2. OFFER (Unicast or Broadcast) ─────────────────│ "You can use 192.168.1.100!"
       │                                                     │
       │ ─── 3. REQUEST (Broadcast) ─────────────────────────>│ "I accept 192.168.1.100."
       │                                                     │
       │ <── 4. ACKNOWLEDGE (ACK) ────────────────────────────│ "Confirmed! Here are DNS & Gateway."
```

- **Lease Time:** An IP is not granted permanently. At 50% of the lease time ($T1$), the client requests renewal.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Complete DNS Resolution](./03-The-Complete-DNS-Resolution-Walkthrough.md) | [README](./README.md) | [05 - Split-Horizon DNS](./05-Split-Horizon-DNS-and-Private-Hosted-Zones.md) |
