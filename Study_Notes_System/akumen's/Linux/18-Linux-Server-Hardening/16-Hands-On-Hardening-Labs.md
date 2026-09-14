# 16. Hands-On Hardening Labs

## Lab 1: SSH Server Hardening
1. Open active SSH session #1 (DO NOT CLOSE THIS).
2. Edit `/etc/ssh/sshd_config`:
   - Set `PermitRootLogin no`
   - Set `PasswordAuthentication no`
3. Test syntax: `sudo sshd -t`
4. Reload service: `sudo systemctl reload ssh`
5. Open SSH session #2 to verify key login works.

## Lab 2: UFW Firewall Setup
1. Reset firewall: `sudo ufw reset`
2. Block incoming: `sudo ufw default deny incoming`
3. Allow outgoing: `sudo ufw default allow outgoing`
4. Allow SSH (Port 22): `sudo ufw allow 22/tcp`
5. Enable firewall: `sudo ufw enable`
6. Confirm status: `sudo ufw status`

## Lab 3: Sysctl Kernel Tuning
1. Create `/etc/sysctl.d/99-security.conf`
2. Add `net.ipv4.tcp_syncookies = 1` and `net.ipv4.ip_forward = 0`
3. Apply settings: `sudo sysctl --system`
4. Verify setting: `sysctl net.ipv4.tcp_syncookies`
