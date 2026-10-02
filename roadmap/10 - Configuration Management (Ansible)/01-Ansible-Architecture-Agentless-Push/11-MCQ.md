# 11 - MCQ & Knowledge Check

#### Q1. Which operating system CANNOT act as a native Ansible Control Node?
- [ ] A) Ubuntu Server 22.04
- [ ] B) macOS Sonoma
- [x] C) Windows Server 2022 (Natively without WSL)
- [ ] D) Red Hat Enterprise Linux 9

<details>
<summary>Explanation</summary>
Ansible control node requires POSIX fork capability. Windows cannot host an Ansible control node natively (though WSL2 works as an emulation layer).
</details>

---

#### Q2. What is the highest priority location for `ansible.cfg`?
- [x] A) Path defined in `ANSIBLE_CONFIG` environment variable
- [ ] B) `./ansible.cfg` in current working directory
- [ ] C) `~/.ansible.cfg` in user home directory
- [ ] D) `/etc/ansible/ansible.cfg`

<details>
<summary>Explanation</summary>
The ANSIBLE_CONFIG environment variable overrides all local or global configuration files.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
