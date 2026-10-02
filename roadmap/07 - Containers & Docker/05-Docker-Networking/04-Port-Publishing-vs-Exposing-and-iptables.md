# 04 - Port Publishing vs Exposing and iptables

## 1. `EXPOSE` vs `-p` Publishing

- **`EXPOSE 8080` (Dockerfile)**: Purely documentation! It does **NOT** publish the port to the host or open any firewall rules.
- **`-p 80:8080` (CLI)**: Actively creates Linux kernel `iptables` NAT rules to forward host port 80 to container port 8080.

---

## 2. The `DOCKER-USER` Security Firewall Chain

By default, Docker manipulates `iptables` directly. When you publish a port (`-p 8080:80`), Docker inserts rules into the `PREROUTING` and `DOCKER` chains **before standard OS firewalls like UFW or firewalld**! This means publishing a port exposes it to the entire public internet, even if UFW is set to deny!

To filter external traffic securely, administrators must place rules in the **`DOCKER-USER`** chain:

```bash
# Block all external traffic to published Docker ports except from trusted IP 203.0.113.50
sudo iptables -I DOCKER-USER -i eth0 -s 203.0.113.50 -j ACCEPT
sudo iptables -A DOCKER-USER -i eth0 -j DROP
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Embedded DNS & Discovery](./03-Embedded-DNS-and-Service-Discovery.md) | [README](./README.md) | [05 - Host, None & Macvlan](./05-Host-None-and-Macvlan-Networking.md) |
