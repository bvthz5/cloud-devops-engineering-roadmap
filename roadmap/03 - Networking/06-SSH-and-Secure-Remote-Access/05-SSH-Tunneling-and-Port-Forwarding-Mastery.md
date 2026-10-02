# 05 — SSH Tunneling and Port Forwarding Mastery

---

## 1. Tunnel Types

- **Local (`-L`):** Forward local port to remote destination.
  ```bash
  ssh -N -L 5432:db.private:5432 user@bastion
  ```
- **Remote (`-R`):** Forward remote port to local development service.
  ```bash
  ssh -N -R 8080:localhost:3000 user@public-vps
  ```
- **Dynamic (`-D`):** SOCKS5 proxy for browser traffic.
  ```bash
  ssh -N -D 1080 user@proxy
  ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Client Config & Bastions](./04-SSH-Client-Configuration-and-Bastions.md) | [README](./README.md) | [06 - SSH Certificates](./06-SSH-Certificates-and-Zero-Trust-Access.md) |
