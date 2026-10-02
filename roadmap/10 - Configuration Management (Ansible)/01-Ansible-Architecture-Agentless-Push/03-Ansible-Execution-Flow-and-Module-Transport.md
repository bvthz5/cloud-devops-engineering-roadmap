# 03 - Ansible Execution Flow & Module Transport

## 1. Deep-Dive Execution Lifecycle

When you run `ansible-playbook -i inventory site.yml`, the engine executes 6 distinct stages for every task:

```text
 [1. Parse Playbook & Inventory]
               │
               ▼
 [2. Synthesize Module Payload (Python Zip)]
               │
               ▼
 [3. SFTP / SCP Copy to Managed Node (~/.ansible/tmp/)]
               │
               ▼
 [4. Execute Payload via Managed Node Python Interpreter]
               │
               ▼
 [5. Parse Output JSON (changed, failed, msg, facts)]
               │
               ▼
 [6. Cleanup Remote Temporary Payload Directory]
```

---

## 2. Inspected Payload Generation

Ansible modules are not transferred as raw `.py` text. They are bundled using `Ansiballz` framework into a single executable Python zip payload containing:
- Module code (`ansible.modules.system.apt`)
- Shared module utility libraries (`ansible.module_utils.basic`)
- Arguments encoded in JSON string (`ANSIBALLZ_PARAMS`)

---

## 3. Controlling Transport & Temp Directories

In `ansible.cfg`:
```ini
[defaults]
remote_tmp = ~/.ansible/tmp
local_tmp  = ~/.ansible/tmp
allow_world_readable_tmp = False

[ssh_connection]
scp_if_ssh = Smart
transfer_method = sftp
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Agentless Architecture and SSH Push Model](./02-Agentless-Architecture-and-SSH-Push-Model.md) | [Index](../../../README.md) | [04 - Ansible Configuration ansible cfg Hierarchy →](./04-Ansible-Configuration-ansible-cfg-Hierarchy.md) |
