# 18. Interview Questions and Answers

1. **What is the difference between DAC and MAC in Linux security?**
   - **DAC (Discretionary Access Control):** Traditional permissions (`chmod`/`chown`) based on owner/group.
   - **MAC (Mandatory Access Control):** System-enforced policies (SELinux/AppArmor) controlling process behavior regardless of user root status.

2. **How do you disable root SSH logins?**
   Set `PermitRootLogin no` in `/etc/ssh/sshd_config` and reload SSH.

3. **What is SUID and why can it be a security risk?**
   SUID causes a binary to execute with file owner permissions (`root`). If flawed, users can leverage it for root privilege escalation.

4. **What does `2>&1` do?**
   Redirects standard error (`stderr`) to standard output (`stdout`).

5. **How does Fail2Ban prevent brute-force attacks?**
   Monitors log files for repeated authentication failures and dynamically adds iptables/nftables firewall rules to drop packets from the attacker's IP.
