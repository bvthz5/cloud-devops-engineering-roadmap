# 09 — SSH Interview Q&A

10 technical interview questions for DevOps, SRE, and Security roles.

---

### Q1: Why is `ProxyJump` preferred over `ssh-agent` forwarding (`ForwardAgent yes`)?
**Answer:**
`ssh-agent` forwarding creates a Unix domain socket on the remote jump host. Any user with root privileges on that jump host can hijack that socket and authenticate to any other server using your credentials. `ProxyJump` merely forwards raw encrypted TCP packets through the jump host; your private keys and credentials never touch the jump host's memory.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
