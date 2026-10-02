# 01 - Ansible Ad-Hoc Command Syntax & Use Cases

## Syntax Structure
```bash
ansible <host-pattern> -m <module_name> -a "<module-arguments>" [options]
```

## Examples
```bash
# Check reboot requirement on web servers
ansible webservers -m command -a "uptime"

# Restart Nginx service
ansible webservers -m service -a "name=nginx state=restarted" --become
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Package & System Modules](./02-System-and-Package-Modules-apt-yum-dnf-service-systemd.md) |
