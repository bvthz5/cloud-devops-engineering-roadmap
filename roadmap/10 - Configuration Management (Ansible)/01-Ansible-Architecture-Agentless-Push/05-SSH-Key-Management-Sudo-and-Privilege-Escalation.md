# 05 - SSH Key Management & Privilege Escalation (Become)

## 1. Privilege Escalation (Become Architecture)

Ansible uses the `become` keyword to execute tasks with elevated privileges (e.g., as `root` or another service account).

```hcl
Playbook Task
   │
   ▼
SSH Connection as 'devops' User
   │
   ▼
Executes: sudo su - root -c "python3 ~/.ansible/tmp/..."
   │
   ▼
Task Executes as Root User
```

---

## 2. Configuring Privilege Escalation Parameters

Can be configured in `ansible.cfg`, playbooks, or CLI:

### In `ansible.cfg`:
```ini
[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = False
```

### In Playbooks:
```yaml
- name: Deploy Web Application
  hosts: webservers
  become: yes
  become_user: root
  tasks:
    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
```

---

## 3. Sudoers Configuration for Ansible User

On managed nodes, configure passwordless sudo for the ansible user (`/etc/sudoers.d/devops`):
```text
devops ALL=(ALL) NOPASSWD: ALL
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Ansible Configuration ansible cfg Hierarchy](./04-Ansible-Configuration-ansible-cfg-Hierarchy.md) | [Index](../../../README.md) | [06 - Ansible Engine vs Core vs Collections Evolution →](./06-Ansible-Engine-vs-Core-vs-Collections-Evolution.md) |
