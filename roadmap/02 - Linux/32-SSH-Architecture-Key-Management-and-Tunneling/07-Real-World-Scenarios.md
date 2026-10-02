# 07 — Real-World SSH Production Scenarios

---

## Scenario 1: Developer Locked Out After Modifying `sshd_config`

### Incident Summary
An admin changes `/etc/ssh/sshd_config`, reloads `sshd`, and closes the terminal. Subsequent connections fail with `Permission denied (publickey)`.

### Root Cause Analysis
The admin set `PasswordAuthentication no` before copying their public key into `/root/.ssh/authorized_keys`, or set incorrect file permissions on `.ssh`.

### Production Solution
1. Access the host via cloud provider serial console or rescue image.
2. Verify permissions:
   ```bash
   chmod 700 /home/ubuntu/.ssh
   chmod 600 /home/ubuntu/.ssh/authorized_keys
   chown -R ubuntu:ubuntu /home/ubuntu/.ssh
   ```
3. Test syntax before restarting:
   ```bash
   sudo sshd -t && sudo systemctl restart sshd
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Agent Forwarding & Security](./06-SSH-Agent-Forwarding-and-Security-Risks.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
