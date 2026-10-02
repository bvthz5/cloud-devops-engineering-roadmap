# 02 - Bridge Networking and Virtual Ethernet Pairs

## 1. How Bridge Networking Operates

When Docker starts a container on a bridge network:
1. Docker creates a **Virtual Ethernet pair (`veth`)**.
2. One end of the pair is plugged into the Linux bridge (e.g. `docker0`).
3. The other end is moved into the container's isolated network namespace and renamed to `eth0`.
4. Outbound packets are translated via **iptables MASQUERADE (SNAT)**.

```text
[ Container Net Namespace ]
      │ (eth0: 172.17.0.2)
      ▼
  [ vethxxxxxx ] (Virtual Cable)
      │
      ▼
[ Linux Bridge (docker0: 172.17.0.1) ] ──► [ iptables NAT (MASQUERADE) ] ──► [ Physical eth0 ] ──► Internet
```

---

## 2. Default `docker0` Bridge vs User-Defined Bridges

| Feature | Default `docker0` Bridge | User-Defined Custom Bridge |
|---|---|---|
| **Automatic DNS Resolution** | **NO** (Must use legacy `--link` flags) | **YES** (Containers resolve by name via 127.0.0.11) |
| **Network Isolation** | All unassigned containers share same subnet | Complete cryptographic and routing isolation |
| **Live Attachment** | Cannot attach/detach while running | Can dynamically connect/disconnect live containers |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Docker Network Architecture](./01-Docker-Network-Architecture-and-Drivers.md) | [README](./README.md) | [03 - Embedded DNS & Discovery](./03-Embedded-DNS-and-Service-Discovery.md) |
