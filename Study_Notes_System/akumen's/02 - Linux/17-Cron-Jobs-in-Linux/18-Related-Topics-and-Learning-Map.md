# 18. Related Topics and Learning Map

Cron connects to several Linux and DevOps topics.

```text
Linux task
   |
   +--> simple schedule --------> cron
   |
   +--> missed-run handling ----> anacron / suitable timer
   |
   +--> service dependencies ---> systemd timers
   |
   +--> container workload -----> Kubernetes CronJob
   |
   +--> complex workflow -------> workflow/orchestration platform
```

## Related Linux & DevOps Topics

### Linux Fundamentals
- Shell Scripting (Bash, exit status, pipes, redirection)
- File Permissions (`chmod`, `chown`, file security)
- Users and Groups (`sudoers`, least privilege)
- Process Management (`ps`, `top`, `kill`, process signals)
- Environment Variables (`PATH`, `SHELL`, `.bashrc`)
- Filesystem Hierarchy Standard (FHS)

### System Administration
- `systemd` services and `.timer` units
- `journalctl` & `syslog` log analysis
- `logrotate` log maintenance
- System monitoring & metrics collection
- Backup & restore automation

### DevOps & Automation
- Configuration Management (Ansible scheduled tasks)
- Infrastructure Automation & CI/CD pipelines
- Container Orchestration (Kubernetes `Jobs` & `CronJobs`)

## Next Skills to Practice
1. Bash Scripting for Automation
2. `systemd` Service and Timer Unit Authoring
3. `logrotate` Configuration
4. Linux Security & Privilege Escalation Audit
5. Kubernetes `CronJob` Manifests

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [17 - Commands Cheat Sheet](./17-Commands-Cheat-Sheet.md) | [README](./README.md) | [README (Index)](./README.md) |
