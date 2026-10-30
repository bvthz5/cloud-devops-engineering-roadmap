# 07 - Real-World Scenarios: Bastion & Jump Host Architectures

## Scenario: Connecting to Private Subnet Managed Nodes via SSH Jump Host

In enterprise environments, managed nodes are placed in private subnets with no public IPs. Ansible control node must tunnel through a Bastion / Jump Host.

```text
[Ansible Control Node] === (Public IP) ===> [Bastion Host] === (Private Subnet) ===> [Target DB Server]
```

---

## Ansible Solution Configuration

In `./ansible.cfg` or `ssh.cfg`:

```ini
[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o ProxyCommand="ssh -W %h:%p -q ubuntu@bastion.example.com"
pipelining = True
```

Alternatively in inventory file `hosts.yml`:
```yaml
all:
  hosts:
    db-server-01:
      ansible_host: 10.0.2.45
      ansible_user: ubuntu
      ansible_ssh_common_args: '-o ProxyJump=ubuntu@bastion.example.com'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Ansible Engine vs Core vs Collections Evolution](./06-Ansible-Engine-vs-Core-vs-Collections-Evolution.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
