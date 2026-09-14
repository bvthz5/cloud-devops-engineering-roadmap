# 1. Hardening Fundamentals

## What is Server Hardening?
**Linux Server Hardening** is the systematic process of securing an operating system by eliminating potential security vulnerabilities and reducing the system's overall **attack surface**.

## Core Principles

### 1. Principle of Least Privilege (PoLP)
Users, applications, and processes should only possess the minimum permissions required to perform their intended function.

### 2. Defense-in-Depth
Relying on a single security layer (e.g., just a password) is insufficient. Security must be implemented in multiple concentric layers:
```text
[ Physical / Cloud Security ]
      ↓
[ Network / Host Firewall ]
      ↓
[ OS Access Control / PAM / SSH ]
      ↓
[ Kernel / MAC (SELinux/AppArmor) ]
      ↓
[ Data Encryption & File Permissions ]
```

### 3. Attack Surface Reduction
Disable or uninstall every unused service, protocol, application, user account, and network port. If a service does not exist, it cannot be exploited.

### 4. Benchmark Standards
Industry benchmarks provide standardized hardening guidelines:
- **CIS Benchmarks** (Center for Internet Security)
- **NIST SP 800-53**
- **DISA STIGs** (Defense Information Systems Agency)
