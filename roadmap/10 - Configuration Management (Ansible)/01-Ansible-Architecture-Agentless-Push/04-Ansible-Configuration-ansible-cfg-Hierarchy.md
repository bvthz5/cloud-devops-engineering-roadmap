# 04 - Ansible Configuration (ansible.cfg) Hierarchy

## 1. Configuration File Search Order

Ansible looks for its configuration file (`ansible.cfg`) in 4 specific locations in strict order of precedence. **First match wins!**

```text
 1. ANSIBLE_CONFIG (Environment Variable)
         │ (if not set)
         ▼
 2. ./ansible.cfg (Current Working Directory)
         │ (if not set)
         ▼
 3. ~/.ansible.cfg (User Home Directory)
         │ (if not set)
         ▼
 4. /etc/ansible/ansible.cfg (Global Default File)
```

---

## 2. Inspecting Active Settings

To check which configuration file is currently active:
```bash
ansible --version
```
Output:
```text
ansible [core 2.15.2]
  config file = /home/devops/project/ansible.cfg
  configured module search path = ['/home/devops/.ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12
```

To view current merged configuration settings:
```bash
ansible-config view   # Displays contents of active ansible.cfg
ansible-config dump   # Displays all active settings and where they came from
```

---

## 3. Production Ansible Configuration Template

```ini
[defaults]
inventory         = ./inventory/hosts.yml
remote_user       = devops
private_key_file  = ~/.ssh/id_ed25519
host_key_checking = True
retry_files_enabled = False
stdout_callback   = yaml
callbacks_enabled = timer, profile_tasks

[privilege_escalation]
become            = True
become_method     = sudo
become_user       = root
become_ask_pass   = False

[ssh_connection]
pipelining        = True
control_path      = %(directory)s/ansible-ssh-%%h-%%p-%%r
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Execution Flow](./03-Ansible-Execution-Flow-and-Module-Transport.md) | [README](./README.md) | [05 - SSH Keys & Sudo](./05-SSH-Key-Management-Sudo-and-Privilege-Escalation.md) |
