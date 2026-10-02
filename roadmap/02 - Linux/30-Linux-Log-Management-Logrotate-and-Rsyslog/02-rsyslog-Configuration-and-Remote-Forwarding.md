# 02 — rsyslog Configuration and Remote Forwarding

`rsyslog` (Rocket-fast System for Log Processing) is the standard syslog daemon on Linux. It can process over one million messages per second and securely forward logs across networks.

---

## 1. Configuration Syntax: Selectors and Actions

In `/etc/rsyslog.conf` or `/etc/rsyslog.d/*.conf`:
```text
facility.severity    action
```

Examples:
```text
# Log all authentication messages to auth.log
auth,authpriv.*                 /var/log/auth.log

# Log all kernel messages with severity warning or higher
kern.warning                    /var/log/kernel-warnings.log

# Discard debug logs (stop processing with ~ or stop)
daemon.debug                    stop
```

---

## 2. Forwarding Logs to a Remote Central SIEM / Collector

To prevent a compromised host from tampering with its own logs, forward logs immediately to a remote central collector:

```text
# Forward all logs over reliable TCP to central SIEM
*.* action(type="omfwd" target="siem.internal.company.com" port="514" protocol="tcp"
            action.resumeRetryCount="-1"
            queue.type="linkedList"
            queue.size="100000"
            queue.saveOnShutdown="on")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Linux Logging Architecture](./01-Linux-Logging-Architecture-and-var-log.md) | [README](./README.md) | [03 - Logrotate Deep Dive](./03-Logrotate-Configuration-and-Retention-Policies.md) |
