# 05 — SSH Tunneling and Port Forwarding

SSH tunneling encapsulates arbitrary TCP traffic inside an encrypted SSH tunnel.

---

## 1. Local Port Forwarding (`-L`)

Forwards a local port on your laptop/workstation to a remote destination via the SSH server:
```text
Laptop (127.0.0.1:5432) === Encrypted SSH ===> Bastion === Plaintext ===> Private RDS DB (10.0.1.20:5432)
```
```bash
# Syntax: ssh -L [local_ip:]local_port:remote_dest_ip:remote_dest_port user@ssh_server
ssh -N -L 5432:10.0.1.20:5432 user@bastion.company.com
```
Now point your local PostgreSQL GUI (DBeaver/pgAdmin) to `localhost:5432`!

---

## 2. Remote / Reverse Port Forwarding (`-R`)

Exposes a local port from your machine onto a remote public server (similar to ngrok):
```bash
# Expose local development server (port 3000) on public server (port 8080)
ssh -N -R 8080:localhost:3000 user@public-vps.com
```

---

## 3. Dynamic SOCKS5 Proxy (`-D`)

Turns your SSH client into a local SOCKS5 proxy, routing all browser traffic through the remote server:
```bash
ssh -N -D 1080 user@remote-proxy.com
```
Configure your web browser to use SOCKS5 proxy `127.0.0.1:1080` to access internal web dashboards securely.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - SSH Client Configuration and Bastion Jump Hosts](./04-SSH-Client-Configuration-and-Bastion-Jump-Hosts.md) | [Index](../../../README.md) | [06 - SSH Agent Forwarding and Security Risks →](./06-SSH-Agent-Forwarding-and-Security-Risks.md) |
