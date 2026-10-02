# 09 - Interview Q&A: Ansible Architecture

### Q1: Why is Ansible called "agentless" and what are the architectural benefits over Puppet/Chef?
**Answer:**
Ansible is agentless because it does not require a background daemon or agent installed on target managed nodes. It connects via standard SSH (for Linux) or WinRM (for Windows), transfers transient Python module payloads, executes them, and cleans up.
**Benefits:**
1. Zero agent memory overhead on target servers.
2. Low operational complexity (no agent upgrade pipelines or PKI cert management for daemons).
3. Secure (relies on battle-tested OpenSSH security model).

---

### Q2: What is `ansible-core` vs `ansible` package?
**Answer:**
- `ansible-core`: Contains the minimal Ansible execution engine, CLI binaries (`ansible-playbook`), and essential `ansible.builtin` modules.
- `ansible`: A community distribution bundle containing `ansible-core` plus over 100 curated collections for cloud (AWS, Azure, GCP), networking, and middleware.

---

### Q3: How do you configure Ansible to connect to servers in a private VPC through a Jump Host?
**Answer:**
Using `ansible_ssh_common_args` with OpenSSH ProxyJump directive:
```yaml
ansible_ssh_common_args: '-o ProxyJump=user@bastion-host.com'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
