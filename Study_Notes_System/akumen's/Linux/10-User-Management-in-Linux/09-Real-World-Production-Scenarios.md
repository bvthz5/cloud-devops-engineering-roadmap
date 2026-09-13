# 09 - Real-World Production Scenarios

In production DevOps environments, user management extends beyond simple user creation commands to encompass security compliance, zero-trust privilege design, system service isolation, and automated CI/CD access control.

---

## 🏭 Scenario 1: Least Privilege for Automation (CI/CD Deployer Account)

### Problem Context
A Jenkins or GitHub Actions self-hosted runner needs to deploy updated application code and restart NGINX services on target Linux web servers. Granting the CI/CD runner full root access (`ALL=(ALL) ALL`) creates a catastrophic security vulnerability if the runner is compromised.

### Solution & Production Implementation

1. **Create dedicated service account without interactive login shell:**
   ```bash
   sudo useradd -r -s /sbin/nologin -d /var/lib/deployer -m deployer
   ```

2. **Configure highly granular passwordless sudo entry (`/etc/sudoers.d/deployer`):**
   ```text
   # /etc/sudoers.d/deployer
   # Restrict deployer user to reloading NGINX and restarting application systemd units ONLY
   deployer ALL=(root) NOPASSWD: /usr/bin/systemctl reload nginx, /usr/bin/systemctl restart app.service
   ```

3. **Set permissions on sudoers drop-in file:**
   ```bash
   sudo chmod 0440 /etc/sudoers.d/deployer
   sudo visudo -c
   ```

---

## 🔒 Scenario 2: PCI-DSS Compliance & Password Expiration Hardening

### Problem Context
Under PCI-DSS Requirement 8, security standards require that human user accounts operating on production servers holding cardholder data enforce 90-day password rotations, lock accounts after 6 failed login attempts, and log off inactive sessions.

### Solution & Production Configuration

1. **Set baseline password aging in `/etc/login.defs`:**
   ```text
   PASS_MAX_DAYS   90
   PASS_MIN_DAYS   1
   PASS_WARN_AGE   14
   ```

2. **Enforce policy on existing user accounts using `chage`:**
   ```bash
   for user in $(awk -F: '$3 >= 1000 && $3 < 60000 {print $1}' /etc/passwd); do
       sudo chage -M 90 -m 1 -W 14 -I 7 "$user"
   done
   ```

3. **Configure automatic SSH session timeout (`TMOUT`) in `/etc/profile.d/timeout.sh`:**
   ```bash
   # Automatically terminate inactive SSH sessions after 15 minutes (900 seconds)
   readonly TMOUT=900
   export TMOUT
   ```

---

## 🐳 Scenario 3: Container Security & Non-Root UID Mapping

### Problem Context
By default, Docker containers run as `root` (UID 0) inside the container namespace. If a container breakout exploit occurs, the attacker gains root privileges on the underlying host operating system.

### Solution & Best Practice
Always create a dedicated non-root system user inside Dockerfiles and bind mounts:

```dockerfile
# Production Dockerfile Security Pattern
FROM node:20-alpine

# Create non-root system group and user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app
COPY --chown=appuser:appgroup . .

# Switch to non-root execution context
USER appuser

EXPOSE 3000
CMD ["node", "server.js"]
```
