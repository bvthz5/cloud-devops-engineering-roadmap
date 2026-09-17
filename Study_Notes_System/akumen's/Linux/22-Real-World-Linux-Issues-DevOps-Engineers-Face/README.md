# Topic 22 — Real-World Linux Issues DevOps Engineers Face

## Objective
Master production Linux troubleshooting, root cause analysis, incident response workflows, and system recovery. Every topic in this study module follows the standardized 9-step incident response architecture:

```text
Scenario ➔ Symptoms ➔ Potential Causes ➔ Diagnostic Commands ➔ Output Analysis ➔ Root Cause Identification ➔ Fix / Mitigation ➔ Verification ➔ Prevention
```

## Module Structure

1. **`01-Linux-Troubleshooting-Mindset-and-Workflow.md`**: Methodical troubleshooting philosophy, Golden Signals of Monitoring (Latency, Traffic, Errors, Saturation), incident communication, triage procedures.
2. **`02-High-CPU-Usage-and-Load-Average-Diagnosis.md`**: Load average vs CPU utilization, CPU states (`us`, `sy`, `wa`, `hi`, `si`), isolating runaway processes (`top`, `htop`, `pidstat`, `mpstat`, `perf`).
3. **`03-Memory-Leaks-OOM-Killer-and-Swap-Exhaustion.md`**: Virtual vs resident memory (`VIRT`, `RES`, `SHR`), kernel Out-Of-Memory (OOM) killer mechanics (`oom_score`, `oom_score_adj`), swap thrashing (`vmstat`, `free`, `dmesg`).
4. **`04-Disk-Space-Exhaustion-and-Deleted-Open-Files.md`**: Disk full troubleshooting (`df -h` vs `du -sh`), unlinked open files holding disk space (`lsof +L1`), process file descriptors, reclaiming storage without reboot.
5. **`05-Inode-Exhaustion-and-Disk-IO-Bottlenecks.md`**: Inode allocation depletion (`df -i`), thousands of small file leaks, I/O wait latency (`iostat -xz`, `iotop`), disk throughput vs IOPS limits.
6. **`06-Permissions-Ownership-and-Path-Resolution-Issues.md`**: File permissions (`chmod`, `chown`, `umask`), ACLs (`getfacl`, `setfacl`), binary `$PATH` resolution errors (`which`, `type -a`), broken symlinks.
7. **`07-Windows-CRLF-Line-Ending-Problems.md`**: Windows vs Linux line endings (`\r\n` vs `\n`), interpreter failure (`/bin/bash^M`), diagnosing and fixing via `dos2unix`, `sed`, `file`, and `.gitattributes`.
8. **`08-Systemd-Service-Failures-and-Crash-Loops.md`**: Systemd unit states, service start limits, debugging failed services (`systemctl status`, `journalctl -u`, `systemd-analyze verify`), core dumps (`coredumpctl`).
9. **`09-Port-Conflicts-and-Socket-Binding-Failures.md`**: Socket binding errors (`EADDRINUSE: address already in use`), identifying process owning port (`ss -tulpn`, `lsof -i`, `fuser`), killing rogue processes safely.
10. **`10-SSH-Connectivity-Auth-and-Config-Troubleshooting.md`**: SSH connection timeouts, `Permission denied (publickey)`, key file permissions (`0600`), verbose debugging (`ssh -vvv`), `sshd_config` validation (`sshd -t`).
11. **`11-Firewall-Security-Groups-and-Network-Blocking.md`**: Local firewalls (`iptables`, `nftables`, `ufw`, `firewalld`), cloud security groups / NACLs, network diagnostics (`nc -zv`, `traceroute`, `tcpdump`).
12. **`12-DNS-Resolution-Failures-and-Diagnostic-Tools.md`**: DNS lookup failures (`nsswitch.conf`, `/etc/resolv.conf`), testing resolvers (`dig +trace`, `nslookup`, `getent hosts`), local DNS caching (`systemd-resolved`).
13. **`13-Production-Deployment-Failures-and-Rollback.md`**: Broken deployments, atomic symlink cutovers, blue-green / canary failure recovery, automated rollback patterns, pre-flight readiness checks.
14. **`14-Docker-Container-Troubleshooting.md`**: Container OOM killed (exit 137), container crash loops, Docker daemon network bridge issues, volume mount permissions, inspection (`docker logs`, `docker inspect`, `docker exec`).
15. **`15-Kubernetes-Pod-CrashLoopBackOff-Troubleshooting.md`**: Debugging `CrashLoopBackOff`, `ImagePullBackOff`, `Pending` states, diagnosing using `kubectl describe`, `kubectl logs --previous`, `kubectl exec`.
16. **`16-Suspicious-Processes-and-Security-Incident-Response.md`**: Identifying malicious processes, cryptominers, unauthorized cron persistence (`/var/spool/cron`, `/etc/cron*`), inspecting suspicious network connections, memory dump preservation.
17. **`17-Universal-Troubleshooting-Toolkit-and-Cheat-Sheet.md`**: Ultimate CLI toolkit lookup table (`sysstat`, `strace`, `lsof`, `tcpdump`, `htop`, `ss`, `journalctl`).
18. **`18-Hands-On-Lab-01-Unlinked-Open-File-Disk-Leak.md`**: Step-by-step hands-on lab: Simulating, diagnosing, and resolving unlinked open file disk space leak.
19. **`19-Hands-On-Lab-02-High-CPU-Spike-Isolation.md`**: Step-by-step hands-on lab: Simulating runaway CPU process, analyzing `top`/`pidstat`, and tuning process priorities (`renice`, `cpulimit`).
20. **`20-Hands-On-Lab-03-OOM-Killer-Analysis.md`**: Step-by-step hands-on lab: Simulating RAM exhaustion, inspecting `/var/log/syslog` OOM score messages, and setting memory limits.
21. **`21-Hands-On-Lab-04-Systemd-Service-Crash-Loop.md`**: Step-by-step hands-on lab: Debugging misconfigured systemd service, fixing environment paths, and reloading daemon.
22. **`22-Hands-On-Lab-05-Port-Binding-Conflict.md`**: Step-by-step hands-on lab: Resolving port 80/8080 conflicts using `ss` and `fuser`.
23. **`23-Hands-On-Labs-06-to-10-SSH-DNS-Docker-K8s.md`**: Hands-on labs 6-10: SSH permissions fix, DNS resolver order fix, Docker volume mount fix, K8s CrashLoopBackOff fix.
24. **`24-Real-World-Scenario-Drills-01-to-07.md`**: 7 complete production outage simulation drills with root cause analysis and post-mortem templates.
25. **`25-DevOps-Troubleshooting-Interview-Questions.md`**: Senior DevOps / SRE troubleshooting interview questions, scenario walk-throughs, and technical answers.
26. **`26-Scenario-Based-MCQs-and-Diagnostic-Quizzes.md`**: 15+ scenario-based diagnostic MCQs with answer keys and technical explanations.
27. **`27-Production-Incident-Response-Checklist.md`**: Production incident response checklist, emergency triage steps, and post-mortem guidelines.
28. **`28-Quick-Revision-Notes.md`**: Flashcard-style takeaways, command cheat tables, and diagnostic shortcuts.
29. **`SOURCE.md`**: Preservation of original source documentation and production incident reference materials.
