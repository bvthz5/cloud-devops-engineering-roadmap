# 04 - User & Security Modules

```bash
# Create user devops and add SSH key
ansible all -m user -a "name=devops shell=/bin/bash groups=sudo" --become
ansible all -m authorized_key -a "user=devops key='{{ lookup('file', '~/.ssh/id_rsa.pub') }}'" --become
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - File and Directory Modules file copy template fetch lineinfile blockinfile](./03-File-and-Directory-Modules-file-copy-template-fetch-lineinfile-blockinfile.md) | [Index](../../../README.md) | [05 - Command Execution Modules command shell raw script →](./05-Command-Execution-Modules-command-shell-raw-script.md) |
