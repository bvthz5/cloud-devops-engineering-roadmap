# 02 - Agentless Architecture & SSH Push Model

## 1. Push Model vs Pull Model

Configuration management tools broadly fall into two execution paradigms: **Push** (Ansible) and **Pull** (Puppet, Chef).

```text
PUSH MODEL (Ansible):
Control Node  ====== SSH Push (On-Demand / Scheduled) ======> Target Nodes

PULL MODEL (Puppet/Chef):
Target Nodes  ====== Polling Daemon (Every 30 mins) ========> Central Server
```

### Architectural Comparison Matrix

| Feature | Ansible (Push) | Puppet / Chef (Pull) |
|---|---|---|
| **Agent Requirement** | Agentless (SSH/WinRM) | Agent Daemon required on every node |
| **Port Requirements** | Outbound 22/5986 from Control Node | Inbound 8140 to Puppet Master |
| **Master Node Load** | High CPU during playbooks, 0 idle load | Constant daemon polling load |
| **Instant Execution** | Immediate (Real-time push) | Delayed until next agent check-in poll |
| **Bootstrap Complexity** | Low (Only SSH keys required) | High (Certificates & agent installation) |

---

## 2. How the SSH Push Connection Works

1. Control Node reads Inventory and Playbook.
2. Control Node establishes SSH connection to target node using OpenSSH or `paramiko`.
3. Control Node transfers Python module payload over SFTP/SCP to `~/.ansible/tmp/`.
4. Control Node executes Python script on target machine via target's Python binary.
5. Module sends JSON response back to Control Node via standard stdout.
6. Control Node deletes temporary files from `~/.ansible/tmp/` and closes SSH channel.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Control Node](./01-Ansible-Control-Node-vs-Managed-Nodes.md) | [README](./README.md) | [03 - Execution Flow](./03-Ansible-Execution-Flow-and-Module-Transport.md) |
