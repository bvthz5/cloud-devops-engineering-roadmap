# 07 — Real-World Package Management Outages & Scenarios

Real-world production case studies where package managers, repository updates, and caching caused enterprise outages.

---

## Scenario 1: Disk Exhaustion via Cached Package Archives

### Problem Statement
A high-throughput PostgreSQL production server on Ubuntu suddenly halts all writes and transactions with:
`PANIC: could not write to log file: No space left on device`.
However, `du -sh /var/lib/postgresql/` reports that the database is only consuming 30 GB of a 100 GB volume!

### Architecture Analysis
- Automated CI/CD scripts and Ansible playbooks were running `apt-get install` and `apt-get upgrade` weekly.
- By default, APT downloads `.deb` archives to **`/var/cache/apt/archives/`** and **never deletes them** unless explicitly instructed!
- Over two years of kernel and software updates, 65 GB of obsolete `.deb` installer packages accumulated in `/var/cache/apt/archives/`, eventually filling the root filesystem to 100%.

### Emergency Remediation & Prevention
```bash
# 1. Immediate Disk Recovery: Purge cached .deb archives
sudo apt clean

# 2. Automatically prevent package caching:
# Create /etc/apt/apt.conf.d/02nocache:
cat << 'EOF' | sudo tee /etc/apt/apt.conf.d/02nocache
Dir::Cache::pkgcache "";
Dir::Cache::srcpkgcache "";
EOF
```

---

## Scenario 2: Expired GPG Key Breaking Global CI/CD Pipeline

### Problem Statement
On a Monday morning, all 140 microservice build pipelines across an enterprise fail simultaneously during the Docker setup step with:
`The following signatures were invalid: EXPKEYSIG 7EA0A9C3F273FCD8 Docker Release <docker@docker.com>`.

### Root Cause
Repository maintainers periodically rotate and expire their public GPG signing keys for security. The company's deployment runners were using an outdated, hardcoded GPG key that expired at midnight on Sunday. Because `gpgcheck` was enabled, APT refused to download packages.

### Engineering Solution
1. **Refresh the GPG Key via Modern Keyrings:**
   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor --yes -o /etc/apt/keyrings/docker.gpg
   ```
2. **CI/CD Best Practice:** Never download raw GPG keys directly inside ephemeral CI runners on every build. Mirror trusted repositories inside internal artifact registries (Nexus or JFrog Artifactory) to protect against external upstream key and network disruptions.

---

## Scenario 3: Alpine Container Segmentation Fault via `musl`

### Problem Statement
A data analytics team containerizes a Python microservice with `pandas` and `scipy` using `FROM alpine:3.19` to achieve a small image size. The container compiles after 25 minutes of building, but immediately crashes in Kubernetes with a Segmentation Fault (`SIGSEGV` - Exit code 139) upon receiving its first API request.

### Root Cause
Scientific Python libraries rely on highly optimized C/Fortran routines compiled specifically against the GNU C Library (**`glibc`**). The lightweight **`musl libc`** implementation in Alpine lacks certain vectorized memory alignment routines expected by the library, triggering a memory segmentation crash.

### Engineering Solution
Migrate from `alpine` to **`debian-slim`**:
```dockerfile
# Replace:
# FROM alpine:3.19
# With:
FROM python:3.11-slim-bookworm

WORKDIR /app
COPY requirements.txt .
# Fast binary wheel installation without compilation:
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```
*Outcome:* Docker build time dropped from 25 minutes to 45 seconds using pre-compiled `manylinux` wheels, and zero segmentation faults occurred in production.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Automated Security Updates and Patching](./06-Automated-Security-Updates-and-Patching.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
