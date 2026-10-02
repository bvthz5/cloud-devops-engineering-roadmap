# 08 - Troubleshooting Architecture & SSH Connectivity

## 1. Common Errors & Resolution Matrix

| Error Message | Root Cause | Remediation |
|---|---|---|
| `UNREACHABLE! => Host key verification failed.` | Target SSH fingerprint not in control node `known_hosts`. | Set `host_key_checking = False` in `ansible.cfg` or run `ssh-keyscan -H target >> ~/.ssh/known_hosts`. |
| `Permission denied (publickey)` | SSH key missing, wrong remote user, or key permissions too open (`0644`). | Run `chmod 600 ~/.ssh/id_ed25519`. Verify `ansible_user` in inventory. |
| `Missing sudo password` | Remote user requires password for `sudo` and `become_ask_pass` is false. | Add `NOPASSWD: ALL` to `/etc/sudoers.d/ansible` or pass `-K` / `--ask-become-pass` CLI flag. |
| `python: command not found` | Managed node lacks Python 2/3 at `/usr/bin/python`. | Set `ansible_python_interpreter=/usr/bin/python3` in inventory. |

---

## 2. Debugging Commands
```bash
# Debug SSH connection with maximum verbosity
ansible all -m ping -vvvv

# Inspect configuration source
ansible-config dump --only-changed
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
