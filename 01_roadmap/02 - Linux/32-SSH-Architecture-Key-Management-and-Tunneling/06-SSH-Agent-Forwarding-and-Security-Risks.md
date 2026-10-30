# 06 — SSH Agent Forwarding and Security Risks

`ssh-agent` stores decrypted private keys in memory so you don't have to retype your passphrase. However, forwarding your agent across intermediate hosts carries severe security risks.

---

## 1. The Agent Forwarding Security Trap (`ForwardAgent yes`)

When you use `ssh -A` or `ForwardAgent yes` to jump through a bastion host:
- The remote server creates a Unix domain socket in `/tmp/ssh-XXXXXX/agent.sock`.
- **Any user with root privileges on that bastion server can hijack your socket** and impersonate you to access all your downstream infrastructure!

---

## 2. The Superior Solution: `ProxyJump`

`ProxyJump` (`-J` or `ProxyJump` in config) establishes an end-to-end encrypted connection directly between your laptop and the final server. The intermediate bastion merely passes raw TCP packets; **your private keys and authentication credentials never touch the bastion memory!**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - SSH Tunneling and Port Forwarding](./05-SSH-Tunneling-and-Port-Forwarding.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
