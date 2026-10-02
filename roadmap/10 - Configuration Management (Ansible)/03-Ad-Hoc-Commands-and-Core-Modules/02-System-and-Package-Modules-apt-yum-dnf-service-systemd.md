# 02 - Package & System Modules (apt, dnf, systemd)

## Package Management
```bash
ansible webservers -m apt -a "name=curl state=present update_cache=yes" --become
```

## Service Management
```bash
ansible webservers -m systemd -a "name=nginx state=started enabled=yes" --become
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Ad-Hoc Syntax](./01-Ansible-Ad-Hoc-Command-Syntax-and-Use-Cases.md) | [README](./README.md) | [03 - File & Directory Modules](./03-File-and-Directory-Modules-file-copy-template-fetch-lineinfile-blockinfile.md) |
