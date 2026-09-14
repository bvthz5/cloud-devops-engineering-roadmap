# 5. SSH Server Hardening — Comprehensive Production Guide

## Executive Summary & Threat Model

Secure Shell (SSH) is the primary remote administration protocol for Linux servers. Because SSH port `22` is exposed to the network, it is constantly targeted by automated botnets performing dictionary brute-force attacks, credential stuffing, and key harvesting.

Hardening SSH requires a multi-layered approach:
1. Eliminating password authentication in favor of modern public-key cryptography.
2. Restricting root logins and limiting user access lists.
3. Tuning cryptographic ciphers, key exchanges (KEX), and MAC algorithms.
4. Implementing session timeouts and connection concurrency limits.
5. Setting up Bastion Jump hosts and certificate-based authentication for enterprise environments.

---

## 1. Public Key Cryptography & Key Management

### Ed25519 vs RSA
- **Ed25519 (Recommended):** Based on Curve25519. Provides high performance, immunity to side-channel timing attacks, and short 68-character key lengths while offering security equivalent to ~3072-bit RSA.
- **RSA 4096:** Acceptable legacy fallback if older systems don't support Ed25519. (Avoid RSA 1024 or 2048).
- **DSA / ECDSA:** Avoid DSA (insecure 1024-bit limit) and ECDSA (potential NIST curve backdoor concerns).

### Key Generation Best Practices
Generate keypair on client machine with 100 KDF rounds:
```bash
ssh-keygen -t ed25519 -a 100 -C "devops-admin@company.com"
```

### Securing `authorized_keys` File Permissions
Strict ownership and permission masks are required on the server, otherwise SSH will reject key authentication (`StrictModes` enforcement):

```bash
# Set permissions on user's home directory and .ssh folder
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chown -R $USER:$USER ~/.ssh
```

---

## 2. Production `/etc/ssh/sshd_config` Configuration

Below is an enterprise-hardened configuration file for `/etc/ssh/sshd_config` or `/etc/ssh/sshd_config.d/50-hardening.conf`:

```ini
# =====================================================================
# NETWORK & BINDING
# =====================================================================
# Change default port to reduce automated brute-force noise
Port 2222

# Explicitly bind to specific IP address (avoid 0.0.0.0 if multi-homed)
ListenAddress 192.168.1.10

# Force IPv4 or IPv6 only
AddressFamily inet

# =====================================================================
# AUTHENTICATION RESTRICTIONS
# =====================================================================
# Disable root login over SSH completely
PermitRootLogin no

# Force Public Key Authentication only (Disable passwords)
PasswordAuthentication no
PubkeyAuthentication yes
PermitEmptyPasswords no

# Two-factor / Multi-step authentication (if using PAM / TOTP)
AuthenticationMethods publickey

# Strict ownership and permission checks on .ssh and authorized_keys
StrictModes yes

# Limit authentication attempts per connection to prevent brute force
MaxAuthTries 3

# Ignore user environment files (~/.ssh/environment)
PermitUserEnvironment no

# =====================================================================
# USER & GROUP ACCESS CONTROL
# =====================================================================
# Only allow specific users or groups to authenticate
AllowGroups sysadmins devops
AllowUsers alice bob@192.168.1.*

# =====================================================================
# SESSION & TIMEOUT MANAGEMENT
# =====================================================================
# Disconnect idle sessions after 5 minutes (300 seconds x 0 tries)
ClientAliveInterval 300
ClientAliveCountMax 0

# Limit simultaneous unauthenticated connections (start:rate:full)
# Drops connections at 30% probability above 10, 100% above 30
MaxStartups 10:30:100

# Limit maximum concurrent open sessions per connection
MaxSessions 2

# =====================================================================
# FEATURE DISABLING (ATTACK SURFACE REDUCTION)
# =====================================================================
# Disable GUI X11 Forwarding
X11Forwarding no

# Disable TCP Port Forwarding (unless required for tunnels/bastions)
AllowTcpForwarding no

# Disable Agent Forwarding (prevents agent socket hijacking)
AllowAgentForwarding no

# Disable TCP KeepAlive (ClientAliveInterval handles this more securely)
TCPKeepAlive no

# Disable banner user information leaks
PrintMotd no
PrintLastLog yes

# Legal Banner Warning
Banner /etc/issue.net

# =====================================================================
# MODERN CRYPTOGRAPHIC CIPHERS, KEX & MACS (CIS BENCHMARK)
# =====================================================================
# Key Exchange Algorithms (KEX)
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,sntrup761x25519-sha512@openssh.com

# Ciphers (Authenticated Encryption with Associated Data - AEAD)
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com

# Message Authentication Code (MACs - Encrypt-then-MAC algorithms)
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com
```

---

## 3. Safe SSH Configuration Reload Procedure

> **CRITICAL WARNING:** Never close your active SSH terminal session while modifying SSH configuration. Open a second terminal window to test authentication after reloading.

```bash
# Step 1: Validate configuration file syntax
sudo sshd -t

# Step 2: Reload SSH daemon (Ubuntu/Debian)
sudo systemctl reload ssh

# Or on RHEL/CentOS:
sudo systemctl reload sshd

# Step 3: Verify service is running cleanly
sudo systemctl status ssh

# Step 4: Open NEW terminal window and test login:
ssh -p 2222 -i ~/.ssh/id_ed25519 alice@server_ip
```

---

## 4. Advanced Enterprise SSH Architecture

### A. SSH Jump Hosts / Bastion Architecture
Avoid exposing internal production servers directly to the internet. Use a Bastion host with `ProxyJump`:

```bash
# In ~/.ssh/config on client machine:
Host bastion
    HostName bastion.company.com
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

Host prod-db-01
    HostName 10.0.1.50
    User admin
    ProxyJump bastion
    IdentityFile ~/.ssh/id_ed25519
```
Connect seamlessly: `ssh prod-db-01`

### B. SSH Certificate Authority (CA) Authentication
Instead of distributing public keys to thousands of servers' `authorized_keys` files:
1. Establish a central SSH Certificate Authority (CA).
2. Sign user public keys with short expiration times (e.g., 8 hours).
3. Servers trust only the CA public key via `TrustedUserCAKeys /etc/ssh/cas.pub`.

---

## 5. Security Audit & Testing Tools

### 1. `ssh-audit` Tool
Scan your SSH server for weak ciphers and compliance:
```bash
# Install and run ssh-audit
sudo apt install ssh-audit -y
ssh-audit -p 2222 localhost
```

### 2. Log Monitoring
Inspect failed SSH authentication attempts in real-time:
```bash
# Debian / Ubuntu
sudo tail -f /var/log/auth.log | grep sshd

# RHEL / CentOS / Systemd Journal
sudo journalctl -u sshd -f | grep Failed
```
