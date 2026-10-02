# 09 — Linux Package Management Interview Q&A

This compilation covers real-world technical interview questions asked for DevOps, SRE, and Cloud Infrastructure roles ranging from Junior to Staff level.

---

### Q1: What is the architectural difference between a low-level package tool and a high-level package manager?
**Answer:**
- **Low-Level Tools (`dpkg`, `rpm`):** Operate directly on local package archive files (`.deb`, `.rpm`). They extract binaries, place configuration files, set permissions, and run pre/post-installation scripts. However, they **do not resolve dependencies**; if a required shared library is missing, installation aborts with an error. They also cannot query remote HTTP/HTTPS mirrors.
- **High-Level Managers (`apt`, `dnf`, `zypper`, `apk`):** Maintain metadata indexes from remote HTTP/HTTPS repositories. They parse dependency graphs, detect conflicts, automatically download missing prerequisite packages in the correct order, verify cryptographic signatures (GPG), and perform atomic transaction rollbacks if configured.

---

### Q2: What happens when you run `apt-get update` versus `apt-get upgrade`?
**Answer:**
- `apt-get update`: Connects to all repositories listed in `/etc/apt/sources.list` and `/etc/apt/sources.list.d/*.sources`, downloads updated `Release` and `Packages.xz` index files, validates their GPG signatures, and updates the local package cache located in `/var/lib/apt/lists/`. **No packages are installed or upgraded.**
- `apt-get upgrade`: Compares currently installed package versions in `/var/lib/dpkg/status` against the newly updated cache and upgrades packages to newer versions **without removing existing packages** or installing new packages that break existing dependencies.
- `apt-get dist-upgrade` (or `apt full-upgrade`): Intelligently handles changing dependencies with new versions of packages, installing new dependencies or deleting obsolete conflicting packages as required.

---

### Q3: Why is `apt-key add` deprecated in modern Debian/Ubuntu systems, and what replaces it?
**Answer:**
- **Why Deprecated:** `apt-key add` placed third-party repository GPG keys into `/etc/apt/trusted.gpg` (a global trust ring). Any key in this global ring was trusted to sign packages for *any* repository configured on the system. If a third-party PPA was compromised, it could publish malicious updates for core OS packages like `libc6` or `openssh-server`, and `apt` would accept them.
- **Modern Replacement:** De-armored GPG keys are saved to `/etc/apt/keyrings/<app>.gpg` with read-only permissions (`0644`). The repository definition explicitly references the key using the `signed-by` attribute:
  ```text
  deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu jammy stable
  ```
  This restricts the key to only validating packages originating from that specific repository URL.

---

### Q4: How does DNF implement transaction rollbacks, and why can't APT natively do this?
**Answer:**
- **DNF / RPM:** DNF utilizes `libsolv` for dependency solving and records every transaction (installed, updated, downgraded, erased packages) in SQLite databases in `/var/lib/dnf/history/`. Because RPM maintains detailed transaction metadata, DNF can perform `dnf history undo <transaction-id>` to revert the exact state.
- **APT / DPKG:** APT does not maintain an atomic rollback transaction engine. While it logs actions to `/var/log/dpkg.log` and `/var/log/apt/history.log`, reversing an upgrade requires manually inspecting logs and downgrading individual packages explicitly using `apt-get install <pkg>=<old-version>`.

---

### Q5: How do you prevent a package from being upgraded during system updates?
**Answer:**
- **Debian / Ubuntu:**
  ```bash
  # Using apt-mark
  sudo apt-mark hold nginx
  # Unhold
  sudo apt-mark unhold nginx
  # Check held packages
  apt-mark showhold
  ```
- **RHEL / Rocky Linux:**
  ```bash
  # Using dnf versionlock plugin
  sudo dnf install 'dnf-command(versionlock)'
  sudo dnf versionlock add nginx
  sudo dnf versionlock list
  ```

---

### Q6: How should package managers be used in Dockerfiles for optimal security and small image sizes?
**Answer:**
1. **Ubuntu / Debian:**
   ```dockerfile
   RUN apt-get update && apt-get install -y --no-install-recommends \
       curl \
       ca-certificates \
    && rm -rf /var/lib/apt/lists/*
   ```
   - Combine `update` and `install` in a single `RUN` layer to prevent caching stale repository indexes.
   - Use `--no-install-recommends` to skip suggested/optional documentation and packages.
   - Delete `/var/lib/apt/lists/*` to keep the layer small.
2. **Alpine Linux:**
   ```dockerfile
   RUN apk add --no-cache curl ca-certificates
   ```
   - `--no-cache` downloads packages directly without persisting index files in `/var/cache/apk/`.
   - Use virtual build groups (`apk add --virtual .build-deps gcc musl-dev ... && apk del .build-deps`) for compilation stages.

---

### Q7: An automated Ansible run fails with: "Could not get lock /var/lib/dpkg/lock-frontend". How do you design your deployment to handle this gracefully?
**Answer:**
1. **Root Cause:** Ubuntu cloud images start `unattended-upgrades` via systemd timers immediately upon boot.
2. **Design Solutions:**
   - In `cloud-init`, configure `package_update: false` and `package_upgrade: false`, or wait for `cloud-init status --wait` before running configuration management.
   - In Ansible playbooks, use retries:
     ```yaml
     - name: Install NGINX
       ansible.builtin.apt:
         name: nginx
         state: present
         lock_timeout: 300 # Retries automatically for up to 5 minutes
     ```
   - In base golden images (Packer), disable auto-upgrades if the images are deployed into immutable auto-scaling groups:
     ```bash
     systemctl disable --now unattended-upgrades.service apt-daily.timer apt-daily-upgrade.timer
     ```

---

### Q8: What is an RPM `.spec` file, and what are its key stages?
**Answer:**
An RPM `.spec` file contains instructions and metadata for building an RPM package.
Key sections include:
1. `Preamble`: Name, Version, Release, License, Source tarball, BuildRequires, Requires.
2. `%prep`: Unpacks source code and applies security patches (`%setup -q`).
3. `%build`: Compiles the binary using `make` or `cmake`.
4. `%install`: Copies compiled files into the build root sandbox (`%{buildroot}`).
5. `%files`: Lists files packaged into the final RPM with permissions and ownership.
6. `%pre`, `%post`, `%preun`, `%postun`: Shell scripts executed before/after install/uninstall.

---

### Q9: How do you verify the cryptographic integrity and file checksums of an installed package to detect tampering?
**Answer:**
- **Debian / Ubuntu:**
  ```bash
  # Verify checksums of installed files against /var/lib/dpkg/info/<pkg>.md5sums
  sudo debsums -s <package-name>
  ```
- **RHEL / Rocky Linux:**
  ```bash
  # RPM compares file sizes, MD5/SHA256 digests, permissions, and timestamps against RPM DB
  sudo rpm -V <package-name>
  # Example output:
  # S.5....T.  c /etc/nginx/nginx.conf (Size, MD5 hash, and Timestamp changed)
  ```

---

### Q10: How do you extract files from a `.deb` or `.rpm` file without installing it?
**Answer:**
- **For `.deb`:**
  ```bash
  # A deb is an 'ar' archive containing control.tar.gz and data.tar.xz
  ar x package.deb
  tar -xf data.tar.xz -C ./extracted/
  # Or directly using dpkg:
  dpkg-deb -x package.deb ./extracted/
  ```
- **For `.rpm`:**
  ```bash
  # Convert rpm to cpio archive and extract
  rpm2cpio package.rpm | cpio -idmv
  ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
