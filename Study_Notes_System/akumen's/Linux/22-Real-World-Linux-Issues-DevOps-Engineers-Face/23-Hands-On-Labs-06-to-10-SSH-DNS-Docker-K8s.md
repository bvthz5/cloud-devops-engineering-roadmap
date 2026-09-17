# 23 — Hands-On Labs 06 to 10: SSH, DNS, Docker, and Kubernetes

## Lab 06: SSH Key Permission Debugging
1. Set SSH key permissions to `chmod 777 ~/.ssh/id_rsa`.
2. Attempt SSH connection (`ssh -i ~/.ssh/id_rsa user@host`). Observe warning: `Permissions 0777 for id_rsa are too open. It is required that your private key files are NOT accessible by others.`
3. Fix permissions using `chmod 600 ~/.ssh/id_rsa` and `chmod 700 ~/.ssh`.

## Lab 07: Testing DNS Resolver Fallback Logic
1. Inspect `/etc/resolv.conf` and `/etc/nsswitch.conf`.
2. Use `dig +trace google.com` to trace root nameserver delegation.
3. Test resolution using `getent hosts google.com`.

## Lab 08: Docker Container Permission & Mount Failure
1. Run container mounting host directory: `docker run -v /root/secret:/app/data ubuntu cat /app/data/file`.
2. Observe permission denied error due to host directory permissions.
3. Adjust permissions (`chmod 755 /root/secret`) or use dedicated Docker volume.

## Lab 09: Kubernetes `CrashLoopBackOff` Investigation
1. Run `kubectl get pods -n dev`.
2. Run `kubectl describe pod <pod-name>`.
3. Extract previous logs using `kubectl logs <pod-name> --previous`.

## Lab 10: Detecting Malware Cron Persistence
1. Inspect `/var/spool/cron/crontabs/` and `/etc/cron.d/`.
2. Check active systemd timers: `systemctl list-timers`.
3. Remove unapproved cron jobs and kill associated background PIDs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [22 - Lab 05: Port Conflict](./22-Hands-On-Lab-05-Port-Binding-Conflict.md) | [README](./README.md) | [24 - Real-World Scenario Drills](./24-Real-World-Scenario-Drills-01-to-07.md) |
