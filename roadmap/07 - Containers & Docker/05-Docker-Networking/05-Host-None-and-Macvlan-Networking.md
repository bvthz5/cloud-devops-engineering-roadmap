# 05 - Host, None, and Macvlan Networking

## 1. `--network host`

In host networking, the container shares the host's network namespace directly:
- **No NAT**: Zero network packet translation overhead (Max possible network performance).
- **Port Conflicts**: If the container binds to port 80, no other service on the host can bind to port 80.

```bash
docker run -d --network host nginx:alpine
```

---

## 2. `--network macvlan`

`macvlan` assigns a dedicated physical MAC address to the container, making it appear as a physical computer connected directly to the corporate router/LAN:

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  corp_lan

# Container receives a real routable LAN IP (192.168.1.55)
docker run -d --net corp_lan --ip 192.168.1.55 myapp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Port Publishing vs Exposing](./04-Port-Publishing-vs-Exposing-and-iptables.md) | [README](./README.md) | [06 - Overlay Networks & VXLAN](./06-Overlay-Networks-and-Multi-Host-VXLAN.md) |
