# 03 - Ansible Execution Flow & Module Transport: The Ansiballz Framework

To master Ansible troubleshooting, performance tuning, and security hardening, engineers must understand what happens beneath the surface of YAML playbooks. Ansible does not run remote shell commands in an ad-hoc loop; it dynamically compiles, packages, and transports self-contained Python programs called **Ansiballz** payloads to target machines over SSH.

---

## 1. The 8-Stage Execution Flow (Under the Hood)

When an engineer issues `ansible-playbook site.yml`, the Ansible engine executes an 8-stage pipeline for every task declared in the play:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    THE 8-STAGE ANSIBLE EXECUTION LIFECYCLE                      │
│                                                                                 │
│   STAGE 1: PLAYBOOK PARSING        STAGE 2: INVENTORY & FACT RESOLUTION         │
│   Reads YAML, resolves roles,      Resolves target hosts, evaluates groups,     │
│   and parses task definitions.     and retrieves cached or discovered facts.    │
│                 │                                             │                 │
│                 ▼                                             ▼                 │
│   STAGE 3: JINJA2 EVALUATION       STAGE 4: ANSIBALLZ PAYLOAD SYNTHESIS         │
│   Interpolates variables and       Assembles module code, module_utils, and     │
│   evaluates template filters.      JSON parameters into an executable zip.      │
│                 │                                             │                 │
│                 ▼                                             ▼                 │
│   STAGE 5: SSH TRANSPORT           STAGE 6: REMOTE PROCESS EXECUTION            │
│   Uploads payload via SFTP/SCP     Target's Python interpreter unpacks the      │
│   to ~/.ansible/tmp/ directory.    zip and runs module code as a subprocess.    │
│                 │                                             │                 │
│                 ▼                                             ▼                 │
│   STAGE 7: JSON CAPTURE            STAGE 8: EPHEMERAL CLEANUP                   │
│   Module outputs result JSON to    Control node deletes remote temporary        │
│   stdout; engine captures diff.    directory; updates terminal / callback logs. │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. The `Ansiballz` Framework: How Payloads Are Assembled

Historically (pre-Ansible 2.1), Ansible used string substitution to inject arguments into module source code. Since modern Ansible, all modules use the **Ansiballz** packaging system:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      ANSIBALLZ PAYLOAD COMPILATION                              │
│                                                                                 │
│   CONTROL NODE MEMORY                                                           │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │ 1. Core Module Code:           ansible/modules/files/copy.py            │   │
│   │ 2. Shared Utilities:           ansible/module_utils/basic.py            │   │
│   │ 3. Arguments Dictionary:       {"dest": "/etc/app.conf", "mode": "0644"}│   │
│   └────────────────────────────────────┬────────────────────────────────────┘   │
│                                        │ Zip Compression & Base64 Wrapping      │
│                                        ▼                                        │
│   SYNTHESIZED EXECUTABLE SCRIPT: `AnsiballZ_copy.py`                            │
│   ┌─────────────────────────────────────────────────────────────────────────┐   │
│   │ #!/usr/bin/python3                                                      │   │
│   │ import zipimport, base64                                                │   │
│   │ ZIPDATA = base64.b64decode("UEsDBBQAAAAIAKV...")                        │   │
│   │ # Mounts in-memory zip bundle without touching disk!                    │   │
│   │ importer = zipimport.zipimporter(temp_zip_file)                         │   │
│   │ importer.load_module('ansible.modules.files.copy').main()               │   │
│   └─────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Why Ansiballz Is High Performance:
- **Zero Remote Dependencies:** The managed node does not need `ansible` installed via pip. All helper libraries, parsers, and utilities travel inside the self-contained zip bundle.
- **Fast Startup:** The module imports from memory using Python's native `zipimport` mechanism, avoiding dozens of disk I/O read operations on the target node.

---

## 3. Remote Directory Lifecycle & Privilege Escalation (`become`)

When the payload reaches the managed node, it undergoes a secure execution lifecycle:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       REMOTE EXECUTION & PRIVILEGE FLOW                         │
│                                                                                 │
│   1. SSH Connection established as unprivileged user: `deployer`                │
│   2. Creates temporary directory: `~/.ansible/tmp/ansible-tmp-171800-847291/`   │
│   3. Directory permissions restricted: `chmod 700`                              │
│   4. SFTP transfers `AnsiballZ_module.py` into this directory                   │
│                                                                                 │
│   IF `become: yes` IS CONFIGURED:                                               │
│   5. Control node issues sudo command:                                          │
│      sudo -H -S -n -u root /bin/sh -c '                                         │
│        /usr/bin/python3 ~/.ansible/tmp/.../AnsiballZ_module.py                  │
│      '                                                                          │
│   6. Python runs with ROOT capabilities, modifying system files                 │
│   7. Module prints JSON output: {"changed": true, "rc": 0} to stdout            │
│   8. Control node immediately issues: `rm -rf ~/.ansible/tmp/ansible-tmp-xxxx/` │
└─────────────────────────────────────────────────────────────────────────────────┘
```

> [!WARNING]
> **The Sudo / Temp File Security Trap:** If the unprivileged user (`deployer`) creates the temporary folder, and `sudo` switches to a different unprivileged user (e.g., `postgres`), the target user might get a `Permission Denied` error trying to read `deployer`'s home directory. Ansible uses POSIX Access Control Lists (`setfacl`) to grant read access dynamically. If ACLs are not installed on the server, Ansible falls back to `allow_world_readable_tmp = False`, which will fail safely rather than exposing credentials to world-readable directories!

---

## 4. Reverse-Engineering: How to Inspect Remote Payloads

By default, Ansible immediately cleans up temporary files after every task. When debugging a custom Python module or troubleshooting unexpected failures, you can force Ansible to preserve the remote script:

```bash
# Set environment variable to keep payloads on target nodes
export ANSIBLE_KEEP_REMOTE_FILES=1

# Run the playbook with verbose output (-vvv)
ansible-playbook -i hosts.ini site.yml -vvv
```

### Inspecting the Payload on the Remote Node:
```bash
# SSH into the managed node
ssh deployer@10.0.1.15

# Navigate to the preserved temporary directory printed in the -vvv logs
cd ~/.ansible/tmp/ansible-tmp-171800-847291/

# View the wrapper script
ls -la
# Output: AnsiballZ_apt.py

# Manually trigger the module to see raw output!
python3 AnsiballZ_apt.py
```
The script will output raw JSON directly to your terminal:
```json
{
  "changed": false,
  "cache_updated": false,
  "invocation": {
    "module_args": {
      "name": "nginx",
      "state": "present"
    }
  },
  "msg": "Package nginx is already installed"
}
```

---

## 5. Transport Configuration in `ansible.cfg`

```ini
[defaults]
# Location on target hosts where temporary modules are transferred
remote_tmp               = ~/.ansible/tmp

# Location on the control node for temporary files
local_tmp                = ~/.ansible/tmp

# Security setting: Prevents world-readable temp files when switching users
allow_world_readable_tmp = False

[ssh_connection]
# Transfer method: sftp (fastest, standard) -> scp (fallback) -> piped
transfer_method          = sftp

# Enables SSH pipelining to pipe module payloads directly into the Python stdin,
# eliminating the SFTP copy stage entirely!
pipelining               = True
```

---

## 6. SRE Summary & Troubleshooting Takeaways

1. **Stdout Must Be Pure JSON:** If a custom bash script, `/etc/profile`, or `.bashrc` echo arbitrary text (e.g., `"Welcome to Ubuntu!"`) during non-interactive logins, it will corrupt Ansible's JSON parser, causing the infamous error:
   ```text
   MODULE FAILURE: JSONDecodeError: Expecting value: line 1 column 1 (char 0)
   ```
2. **Ensure Clean Non-Interactive Shells:** Ensure `.bashrc` exits immediately for non-interactive shells (`[ -z "$PS1" ] && return`).
3. **Verify Disk Space on `/tmp` and `~`:** If target servers run out of disk space or inodes in `/tmp` or the user's home directory, payload upload fails immediately.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Agentless Architecture and SSH Push Model](./02-Agentless-Architecture-and-SSH-Push-Model.md) | [Index](../../../README.md) | [04 - Ansible Configuration ansible cfg Hierarchy →](./04-Ansible-Configuration-ansible-cfg-Hierarchy.md) |

