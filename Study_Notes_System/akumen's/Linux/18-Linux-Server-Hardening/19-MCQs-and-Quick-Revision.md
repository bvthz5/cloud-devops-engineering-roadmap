# 19. MCQs and Quick Revision

## MCQs

1. Which `/etc/ssh/sshd_config` parameter disables password authentication?
   - A. `AllowPassword no`
   - B. `PasswordAuthentication no`
   - C. `DisablePassword yes`
   - D. `AuthType keyonly`
   **Answer: B**

2. Which command inspects active listening ports?
   - A. `ss -tulpn`
   - B. `ps aux`
   - C. `top`
   - D. `systemctl list-ports`
   **Answer: A**

3. What permission bit should `/etc/shadow` ideally have?
   - A. `777`
   - B. `644`
   - C. `600`
   - D. `755`
   **Answer: C**

## Quick Revision
- **Principle of Least Privilege:** Minimal access required.
- **SSH Hardening:** Disable root login, disable passwords, use SSH keys.
- **Firewall:** Default deny incoming.
- **Kernel Tuning:** Enable `tcp_syncookies`, disable `ip_forward`.
