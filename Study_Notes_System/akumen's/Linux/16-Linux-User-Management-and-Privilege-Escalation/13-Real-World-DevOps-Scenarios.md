# 13 - Real-World DevOps Scenarios

---

## 🟢 Scenario 1: Onboarding a New Developer

**The Requirement:** A new developer, Bob, has joined the team. He needs SSH access to the server, must be part of the `devteam` and `docker` groups, and needs his password to expire in 90 days.

**The Commands:**
```bash
# 1. Create the user with home directory, bash shell, and groups
sudo useradd -m -s /bin/bash -c "Bob Johnson" -G devteam,docker bob

# 2. Set an initial password
sudo passwd bob

# 3. Force Bob to change this temporary password on first login
sudo chage -d 0 bob

# 4. Set 90-day password expiry
sudo chage -M 90 bob

# 5. (Optional) Add Bob's SSH public key
sudo mkdir -p /home/bob/.ssh
sudo cp /tmp/bob_id_rsa.pub /home/bob/.ssh/authorized_keys
sudo chown -R bob:bob /home/bob/.ssh
sudo chmod 700 /home/bob/.ssh
sudo chmod 600 /home/bob/.ssh/authorized_keys
```

---

## 🔴 Scenario 2: Offboarding an Employee (Immediate Termination)

**The Requirement:** Alice has been terminated. Her access must be revoked immediately, but her home directory must be preserved for 30 days for HR compliance review.

**The Commands:**
```bash
# 1. Lock the account immediately (disables password login)
sudo usermod -L alice

# 2. Set the account to expire RIGHT NOW
sudo chage -E 0 alice

# 3. Kill all of Alice's running processes
sudo pkill -u alice

# 4. Remove her from all supplementary groups
sudo usermod -G "" alice

# 5. Change her shell to nologin (blocks SSH key access too)
sudo usermod -s /sbin/nologin alice

# 6. Archive her home directory
sudo tar -czvf /backups/alice_home_$(date +%Y%m%d).tar.gz /home/alice

# LATER (after 30 days): Delete the user and home
# sudo userdel -r alice
```

---

## 🟡 Scenario 3: CI/CD Service Account for Ansible

**The Requirement:** An Ansible control node needs to SSH into target servers and run `sudo` commands without a password to deploy applications.

**The Commands (on each target server):**
```bash
# 1. Create the service account
sudo useradd --system --no-create-home --shell /sbin/nologin ansible

# 2. Create a sudoers drop-in file (passwordless, scoped)
sudo visudo -f /etc/sudoers.d/ansible
```
*Content of the drop-in file:*
```text
ansible   ALL=(root)   NOPASSWD: /usr/bin/systemctl, /usr/bin/apt, /usr/bin/docker
```
```bash
# 3. Set up SSH key authentication
sudo mkdir -p /home/ansible/.ssh  # Create if needed for authorized_keys
sudo cp /tmp/ansible_id_rsa.pub /home/ansible/.ssh/authorized_keys
sudo chown -R ansible:ansible /home/ansible/.ssh
sudo chmod 700 /home/ansible/.ssh
sudo chmod 600 /home/ansible/.ssh/authorized_keys

# 4. Override the nologin shell for SSH key access only
sudo usermod -s /bin/bash ansible
```
*(Alternatively, keep `/sbin/nologin` and use Ansible's `command` module which does not require an interactive shell on some setups).*

---

## 🟠 Scenario 4: Shared Server — Isolating Teams

**The Requirement:** Two teams (frontend and backend) share a server. Each team should be able to read/write only their own application directories.

**The Commands:**
```bash
# 1. Create groups
sudo groupadd frontend
sudo groupadd backend

# 2. Assign users
sudo usermod -aG frontend dev1
sudo usermod -aG backend dev2

# 3. Set directory ownership
sudo chown -R root:frontend /opt/frontend-app
sudo chown -R root:backend /opt/backend-app

# 4. Set SGID + permissions (775)
sudo chmod -R 2775 /opt/frontend-app
sudo chmod -R 2775 /opt/backend-app
```
