# 19 - Related Topics

Once you have mastered standard user management and `sudo`, these advanced topics extend identity, authentication, and authorization in enterprise environments.

---

## 🔐 PAM (Pluggable Authentication Modules)

**What it is:** A framework that decouples applications from authentication methods. Instead of each program (SSH, `login`, `su`) implementing its own password checking, they all delegate to PAM.
**Why it matters:** PAM is how you enforce password complexity (`pam_pwquality`), limit login attempts (`pam_faillock`), and require multi-factor authentication. It is configured via files in `/etc/pam.d/`.

---

## 🗝️ SSH Key-Based Authentication

**What it is:** Replacing passwords with public/private key pairs for remote login.
**Why it matters:** In production DevOps environments, password-based SSH login is almost always disabled. Instead, administrators deposit their public key in `~/.ssh/authorized_keys`. This eliminates the risk of brute-force password attacks.

---

## 📋 Access Control Lists (ACLs)

**What it is:** Extending standard `rwx` permissions to grant granular access to specific users or groups beyond the single User/Group/Others model.
**Why it matters:** If Alice needs read access and Bob needs write access to the same file, standard permissions cannot express this. ACLs (`setfacl`, `getfacl`) can.

---

## 🛡️ SELinux / AppArmor (Mandatory Access Control)

**What it is:** Kernel security modules that enforce security policies beyond `sudo` and standard permissions.
**Why it matters:** Even if a user has `sudo ALL`, SELinux or AppArmor can prevent them from accessing resources that violate the security policy. This is a defense-in-depth layer that limits the blast radius of a compromised root session.

---

## 🧩 Linux Capabilities

**What it is:** Breaking the monolithic `root` privilege into discrete, grantable capabilities.
**Why it matters:** Instead of giving a web server SUID root to bind to port 80, you grant it only `CAP_NET_BIND_SERVICE`. This drastically reduces the attack surface if the web server is compromised.

---

## 🏢 LDAP / Active Directory / SSSD

**What it is:** Centralized identity management systems that store user accounts, groups, and passwords in a directory service rather than in local files.
**Why it matters:** In enterprise environments with hundreds of servers, you cannot manage `/etc/passwd` on each machine individually. SSSD (System Security Services Daemon) allows Linux servers to authenticate users against a central Active Directory or LDAP directory, providing single sign-on (SSO).

---

## ☁️ Cloud IAM (AWS IAM, GCP IAM, Azure AD)

**What it is:** Identity and Access Management in cloud platforms.
**Why it matters:** In modern DevOps, user management extends beyond the Linux server. Cloud IAM policies control who can create servers, access databases, or deploy applications. The same principle of least privilege applies: grant only the minimum cloud permissions required.

---

## 🔒 CI/CD Pipeline Security

**What it is:** Securing the automated deployment pipeline.
**Why it matters:** CI/CD runners (Jenkins agents, GitLab runners) often have powerful sudo access on target servers. If the CI/CD system is compromised, the attacker inherits those permissions. Best practices include using short-lived credentials, rotating secrets, and running pipeline steps in isolated containers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [18 - Quick Revision](./18-Quick-Revision.md) | [README](./README.md) | [README (Index)](./README.md) |
