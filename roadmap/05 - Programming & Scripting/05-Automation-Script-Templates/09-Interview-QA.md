# 09 - Automation Templates: Interview Questions & Answers

### Q1: How do you prevent overlapping execution of long-running cron scripts?
**Answer:** By wrapping script execution with Linux `flock` (file lock). Using `flock -n /path/to/lockfile command` attempts to acquire a non-blocking lock; if a previous instance is still executing, the new instance immediately exits without running.

### Q2: Why is testing certificate expiration at 30 days standard in production monitoring?
**Answer:** Automated renewal tools like Let's Encrypt / Certbot trigger renewals 30 days before expiration. If a certificate has $< 30$ days remaining, it indicates that the automated renewal daemon or ACME challenge has silently failed, providing human SREs 30 days of buffer to intervene before customer outages occur.

### Q3: What is the risk of force-deleting pods in an automated remediation script?
**Answer:** Force deleting with `--grace-period=0 --force` immediately removes the Pod from the Kubernetes API server without waiting for the container runtime to flush in-flight database transactions or release file locks, which can cause database corruption in stateful workloads.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
