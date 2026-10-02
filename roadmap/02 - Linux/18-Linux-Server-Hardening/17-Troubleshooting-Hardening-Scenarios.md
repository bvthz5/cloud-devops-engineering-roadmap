# 17. Troubleshooting Hardening Scenarios

## Scenario 1: Locked out of SSH after configuration change
- **Cause:** Firewall port missing or password auth disabled before adding SSH key.
- **Resolution:** Access system via Cloud Console / IPMI / VNC console, fix `/etc/ssh/sshd_config`, run `sudo sshd -t`, and restart service.

## Scenario 2: Fail2Ban locked out legitimate administrator IP
- **Resolution:** Connect via secondary interface/console and unban administrator IP:
  ```bash
  sudo fail2ban-client set sshd unbanip ADMIN_IP_ADDRESS
  ```

## Scenario 3: Application blocked by SELinux
- **Investigation:** Check audit logs for SELinux AVC denials:
  ```bash
  sudo ausearch -m avc -ts recent
  ```
- **Resolution:** Use `audit2allow` to generate custom policy module.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - Hands On Hardening Labs](./16-Hands-On-Hardening-Labs.md) | [README](./README.md) | [18 - Interview Questions and Answers](./18-Interview-Questions-and-Answers.md) |
