# 02 — APT and DPKG Deep Dive: Debian and Ubuntu Systems

---

## 1. The Debian Package Ecosystem

On Debian, Ubuntu, and Debian-derived distributions, package management is split between **DPKG** (the low-level package archive manager) and **APT** (Advanced Package Tool, the high-level dependency-resolving package manager).

```text
User Command: apt install nginx
                     │
                     ▼ Queries Local Package Cache
        /var/lib/apt/lists/ (Downloaded during 'apt update')
                     │
                     ▼ Resolves Dependencies & Downloads .deb
        /var/cache/apt/archives/nginx_1.24.0.deb
                     │
                     ▼ Delegates Installation
        dpkg -i /var/cache/apt/archives/nginx_1.24.0.deb
                     │
                     ▼ Unpacks Files & Runs Scripts
        /usr/sbin/nginx, /etc/nginx/, postinst script
                     │
                     ▼ Updates Local Installed Database
        /var/lib/dpkg/status
```

---

## 2. Repository Configuration: `sources.list` Syntax

APT reads repository locations from `/etc/apt/sources.list` and files inside `/etc/apt/sources.list.d/*.list` (or modern deb822 `.sources` format):

```text
deb [signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu noble stable
 │                 │                                      │                    │      │
 ├─ Type           ├─ Cryptographic Verification          ├─ Repository URI    │      └─ Component
 (deb or deb-src)     Public Key Location                                      └─ Distribution / Suite
```

### Ubuntu Repository Components Explained:
- **`main`:** Officially supported, open-source software maintained directly by Canonical.
- **`restricted`:** Proprietary device drivers (e.g., Nvidia GPU drivers).
- **`universe`:** Community-maintained open-source software (millions of community packages).
- **`multiverse`:** Software restricted by patent or legal copyright issues.

---

## 3. Modern Best Practice: Adding Third-Party Repositories Securely

> **DEPRECATION WARNING:** The legacy command `apt-key add` has been officially deprecated because it placed third-party GPG keys into a global trusted keyring (`/etc/apt/trusted.gpg`), allowing any third-party repository to sign packages for any other package on your system!

### Modern Secure Method (Using Dedicated Keyrings):
```bash
# 1. Download third-party GPG key and de-armor into dedicated keyring directory
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# 2. Add repository entry referencing the dedicated GPG key via signed-by
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 3. Update index and install package
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

---

## 4. `apt` vs `apt-get`: When to Use Which?

- **`apt`:** Designed for **interactive terminal use** by humans. Provides clean progress bars, colorized output, package counts, and combined commands.
- **`apt-get`:** Designed for **automated scripts and CI/CD pipelines**. Guarantees backward compatibility and predictable output format without changing CLI semantics.

```bash
# In CI/CD Scripts, ALWAYS use non-interactive mode:
DEBIAN_FRONTEND=noninteractive apt-get update -y && \
DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends nginx
```

---

## 5. Essential APT Command Reference

| Action | Command | DevOps / SRE Purpose |
| :--- | :--- | :--- |
| **Update Index** | `sudo apt update` | Downloads fresh package lists and checksums from remote mirrors. |
| **Install Package** | `sudo apt install <pkg>` | Downloads, resolves dependencies, and installs software. |
| **Remove Package** | `sudo apt remove <pkg>` | Uninstalls package binaries, but leaves configuration files intact in `/etc/`. |
| **Purge Package** | `sudo apt purge <pkg>` | **Completely deletes** binaries AND all `/etc/` configuration files! |
| **Clean Unused Deps**| `sudo apt autoremove` | Removes orphaned dependencies that are no longer needed by any installed software. |
| **Clean Local Cache**| `sudo apt clean` | Deletes all cached `.deb` installer archives from `/var/cache/apt/archives/` to free disk space. |
| **Inspect Metadata** | `apt show <pkg>` | Displays package description, dependencies, version, and download size. |
| **Search Packages** | `apt search <keyword>` | Searches package names and descriptions in local index. |
| **List Installed** | `apt list --installed` | Lists all software currently installed on the host. |

---

## 6. Low-Level Package Operations with `dpkg`

```bash
# 1. Install a locally downloaded .deb file
sudo dpkg -i package.deb

# 2. List all installed packages
dpkg -l | grep nginx

# 3. List all physical files installed on disk by a package
dpkg -L nginx

# 4. Find which package owns a specific file on the filesystem
dpkg -S /usr/sbin/nginx
# Output: nginx-core: /usr/sbin/nginx

# 5. Extract contents of a .deb file without installing it
dpkg-deb -x package.deb /tmp/extracted/
```

---

## 7. APT Pinning & Priority Control (`/etc/apt/preferences`)

In mixed or multi-repository setups, **APT Pinning** allows you to force a specific version of a package, prioritize one repository over another, or prevent a critical package (like the Linux kernel or PostgreSQL) from being upgraded during general system updates.

Save to `/etc/apt/preferences.d/pin-postgres`:
```ini
Package: postgresql*
Pin: version 15.*
Pin-Priority: 1001
```
- Pin priority `> 1000`: Package will be downgraded if necessary to match the pin.
- Pin priority `500`: Standard default repository priority.
- Pin priority `< 0`: Prevents the package from ever being installed.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Package Management Fundamentals and Package Types](./01-Package-Management-Fundamentals-and-Package-Types.md) | [README](./README.md) | [03 - YUM DNF and RPM Deep Dive RHEL CentOS Rocky](./03-YUM-DNF-and-RPM-Deep-Dive-RHEL-CentOS-Rocky.md) |
