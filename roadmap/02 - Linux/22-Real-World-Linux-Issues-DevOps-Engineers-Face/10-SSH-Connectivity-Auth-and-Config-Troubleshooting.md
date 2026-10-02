# 10 — SSH Connectivity, Auth, and Config Troubleshooting

## 1. Scenario
An automated CI/CD pipeline or admin SSH attempt fails with:
`Permission denied (publickey)` or `ssh: connect to host 192.168.1.50 port 22: Connection timed out`

```text
Scenario
   ↓
Symptoms: Permission denied (publickey), Connection timed out, Connection refused
   ↓
What could cause it? Over-permissive ~/.ssh file permissions, public key missing from authorized_keys, sshd blocking
   ↓
Diagnostic commands: ssh -vvv user@host, tail -n 50 /var/log/auth.log, sshd -t
   ↓
Fix / mitigate: Fix SSH directory permissions (chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys)
```

## 2. SSH Permission Matrix Rule

OpenSSH strictly rejects public key authentication if user home directory or `.ssh` folder permissions are too loose (readable/writable by group or others)!

| Path | Required Permission | Command Fix |
|---|---|---|
| User Home (`~`) | `755` or `700` | `chmod 755 ~` |
| `.ssh` directory (`~/.ssh`) | `700` | `chmod 700 ~/.ssh` |
| `authorized_keys` (`~/.ssh/authorized_keys`) | `600` | `chmod 600 ~/.ssh/authorized_keys` |
| Private Key (`id_ed25519` / `id_rsa`) | `600` | `chmod 600 ~/.ssh/id_ed25519` |

## 3. Debugging with Verbose Mode (`ssh -vvv`)

```bash
ssh -vvv -i ~/.ssh/id_ed25519 user@192.168.1.50
```

### Verbose Log Trace Analysis
```text
debug1: Offering public key: /home/ubuntu/.ssh/id_ed25519 ED25519 explicit
debug1: Authentications that can continue: publickey
debug1: No more authentication methods to try.
Permission denied (publickey).
```
> Cause: Key offered by client was not present in target user's `~/.ssh/authorized_keys` file!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Port Conflicts](./09-Port-Conflicts-and-Socket-Binding-Failures.md) | [README](./README.md) | [11 - Firewall & Security Groups](./11-Firewall-Security-Groups-and-Network-Blocking.md) |
