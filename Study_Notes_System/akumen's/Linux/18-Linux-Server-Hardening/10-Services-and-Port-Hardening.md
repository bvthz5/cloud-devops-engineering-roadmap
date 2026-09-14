# 10. Services and Port Hardening

## Audit Active Listening Ports
Identify all services accepting network connections:
```bash
sudo ss -tulpn
```
Inspect output for unauthorized services listening on `0.0.0.0` or `::`.

## Disable Unnecessary Services
Unneeded legacy or helper services expand the attack surface.

```bash
# Stop and disable unnecessary services (examples)
sudo systemctl stop avahi-daemon bluetooth cups
sudo systemctl disable avahi-daemon bluetooth cups
sudo systemctl mask avahi-daemon
```

## Localhost Binding
Configure non-public services (e.g., MySQL, PostgreSQL, Redis) to listen only on `127.0.0.1` rather than public interfaces.

Example `/etc/mysql/mariadb.conf.d/50-server.cnf`:
```ini
bind-address = 127.0.0.1
```
