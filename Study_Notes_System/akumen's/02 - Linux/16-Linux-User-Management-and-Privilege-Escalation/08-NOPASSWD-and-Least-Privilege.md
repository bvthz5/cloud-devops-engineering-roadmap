# 08 - NOPASSWD and Least Privilege

`NOPASSWD` allows a user to execute `sudo` commands without being prompted for their password. While convenient, it must be used with extreme care following the **Principle of Least Privilege**.

---

## 🔓 The `NOPASSWD` Directive

### Syntax
```text
user   ALL=(run_as_user)   NOPASSWD: /path/to/command
```

### Example: Allow 'deploy' to Restart Nginx Without a Password
```text
deploy   ALL=(root)   NOPASSWD: /usr/bin/systemctl restart nginx
```

### Example: Allow 'deploy' Full Passwordless Root (DANGEROUS)
```text
deploy   ALL=(ALL)   NOPASSWD: ALL
```

---

## 🎯 When is NOPASSWD Appropriate?

| Use Case | Appropriate? | Reason |
| :--- | :---: | :--- |
| **CI/CD Service Accounts** | ✅ Yes | Automated pipelines (Jenkins, GitLab CI runners) cannot type passwords interactively. |
| **Ansible / Configuration Management** | ✅ Yes | Remote SSH execution needs passwordless escalation for playbooks. |
| **Specific Operational Commands** | ✅ Yes | Allowing an on-call engineer to restart a specific service without friction. |
| **Full `ALL` for a Human User** | ❌ No | Removes the last confirmation barrier. If their session is hijacked, the attacker gets instant root. |

---

## 🛡️ The Principle of Least Privilege

The fundamental security principle: **Grant only the minimum permissions required to perform the job.**

### Bad Example (Overly Broad)
```text
deploy   ALL=(ALL)   NOPASSWD: ALL
```
*If the `deploy` account is compromised, the attacker has instant, passwordless root access to the entire server.*

### Good Example (Scoped)
```text
deploy   ALL=(root)   NOPASSWD: /usr/bin/systemctl restart nginx, /usr/bin/systemctl restart myapp, /usr/bin/docker pull *
```
*The `deploy` user can only restart two specific services and pull Docker images. Even if the account is compromised, the attacker cannot read `/etc/shadow`, add users, or install backdoors.*

---

## ⚠️ Combining NOPASSWD with Password-Required Rules

You can mix `NOPASSWD` and `PASSWD` on the same line. The **last matching rule wins**.

```text
alice   ALL=(ALL)   ALL, NOPASSWD: /usr/bin/systemctl restart nginx
```
*This means Alice needs a password for everything, EXCEPT restarting Nginx.*

### Command Deny (`!`)
You can also deny specific commands.
```text
alice   ALL=(ALL)   ALL, !/usr/bin/passwd root, !/usr/sbin/visudo
```
*Alice can run anything via sudo except changing the root password or editing sudoers.*
*(Note: Deny rules are easily bypassed by copying the binary to another location. They are a speed bump, not a wall).*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - sudoers d Modular Rules](./07-sudoers-d-Modular-Rules.md) | [README](./README.md) | [09 - Password Management and Aging](./09-Password-Management-and-Aging.md) |
