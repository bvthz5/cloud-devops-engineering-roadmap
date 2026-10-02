# 06 — SSH Certificates and Zero-Trust Access

Managing static `authorized_keys` files across 1,000 servers is unmanageable. Modern enterprises use **SSH Certificates**:
- A trusted SSH CA issues short-lived (e.g. 8-hour) signed certificates to engineers upon logging in via Single Sign-On (SSO).
- Servers only trust the CA's public key; no individual keys need to be installed on target servers!
- Implemented with **HashiCorp Vault** or **Teleport**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - SSH Tunneling](./05-SSH-Tunneling-and-Port-Forwarding-Mastery.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
