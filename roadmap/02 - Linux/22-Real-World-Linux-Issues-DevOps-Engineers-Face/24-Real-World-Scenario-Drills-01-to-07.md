# 24 — Real-World Scenario Drills 01 to 07

## Drill 01: The Midnight Disk Full Panic
- **Scenario**: At 02:00 AM, PagerDuty triggers alert for web-01: Disk 100% full. Engineers run `rm -rf /var/log/app.log`, but `df -h` still shows 100% full.
- **Root Cause**: `app.log` file handle is held open by Java application PID 2104.
- **Fix**: `:> /proc/2104/fd/4` followed by graceful service restart.

## Drill 02: The Mysterious 502 Bad Gateway
- **Scenario**: Nginx returns 502 Bad Gateway after server reboot.
- **Root Cause**: PHP-FPM socket path `/var/run/php/php7.4-fpm.sock` permissions changed on boot.
- **Fix**: Update `listen.mode = 0660` in `/etc/php/7.4/fpm/pool.d/www.conf`.

## Drill 03: The Silent OOM Massacre
- **Scenario**: Database service `postgresql` disappears every Sunday at 04:00 AM during automated data index jobs.
- **Root Cause**: Memory limit exceeded during index building; kernel OOM killer terminates `postgres`.
- **Fix**: Increase system swap space, reduce `work_mem` parameter in `postgresql.conf`, and set `oom_score_adj`.

## Drill 04: Windows Git Checkout Script Execution Failure
- **Scenario**: CI/CD build worker fails executing shell script checked out from Windows developer laptop.
- **Root Cause**: Windows CRLF line endings (`/bin/bash^M: bad interpreter`).
- **Fix**: `dos2unix build.sh` and commit `.gitattributes`.

## Drill 05: Kubernetes Pod Ingress Host Resolution Failure
- **Scenario**: Microservice inside K8s cluster cannot reach external third-party API endpoint.
- **Root Cause**: CoreDNS resolver upstream forwarding timeout.
- **Fix**: Update CoreDNS ConfigMap upstream DNS list to public resolvers.

## Drill 06: High Load Average with Low CPU Usage
- **Scenario**: Load average is 24.0 on an 8-core server, but top shows 90% idle CPU.
- **Root Cause**: High I/O wait (`%wa`) due to failing SAN storage device or slow disk write queuing.
- **Fix**: Identify bottleneck process with `iotop -oPa` and replace failing storage drive.

## Drill 07: Unresponsive SSH Server
- **Scenario**: Admin cannot SSH into server (`ssh: connect to host port 22: Connection refused`).
- **Root Cause**: UFW firewall rule or `sshd_config` Port configuration error.
- **Fix**: Console access via cloud provider serial console, inspect `journalctl -u sshd`, fix port binding, and reload firewall.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [23 - Hands-On Labs 06-10](./23-Hands-On-Labs-06-to-10-SSH-DNS-Docker-K8s.md) | [README](./README.md) | [25 - DevOps Interview Q&A](./25-DevOps-Troubleshooting-Interview-Questions.md) |
