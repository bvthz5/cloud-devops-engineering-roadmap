# 06 - Ansible Engine vs Core vs Collections Evolution

## 1. The Ansible Packaging Split (Ansible 2.9 to 2.10+)

Prior to Ansible 2.10, all modules were contained in a single massive repository (`ansible/ansible`). Starting in 2.10, Ansible split into two distinct entities:

```text
                  ANSIBLE ECOSYSTEM
                 ───────────────────
  +-------------------------------------------------+
  | 'ansible' (Community Package / Battery Included) |
  | Includes ansible-core + ~100+ Collections      |
  +------------------------+------------------------+
                           |
                           v
  +-------------------------------------------------+
  | 'ansible-core' (Minimal Core Engine)            |
  | Contains ansible CLI binaries, playbook parser,|
  | & built-in modules (ansible.builtin.*)          |
  +-------------------------------------------------+
```

---

## 2. Package Comparison Matrix

| Package Name | Content | Typical Installation |
|---|---|---|
| `ansible-core` | Core CLI tools (`ansible`, `ansible-playbook`), `ansible.builtin` collection | `pip install ansible-core` |
| `ansible` | `ansible-core` + curated community collections (AWS, Azure, GCP, POSIX, etc.) | `pip install ansible` |

---

## 3. Collection Namespace Concept

Collections use Fully Qualified Collection Names (FQCN):
- `ansible.builtin.copy`
- `amazon.aws.ec2_instance`
- `community.general.htpasswd`

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - SSH Keys & Sudo](./05-SSH-Key-Management-Sudo-and-Privilege-Escalation.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
