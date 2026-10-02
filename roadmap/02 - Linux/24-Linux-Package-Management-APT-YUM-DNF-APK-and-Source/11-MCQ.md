# 11 — Multiple Choice Questions (Self-Assessment)

Test your understanding of Linux package management concepts, command options, and troubleshooting procedures.

---

### Q1. What does the command `apt-get install -f` do?
- [ ] A) Forces the installation of a package even if GPG signature verification fails
- [ ] B) Fixes broken dependencies by attempting to correct an unsatisfied dependency tree
- [ ] C) Fast-tracks installation by skipping pre-installation scripts
- [ ] D) Formats the package cache directory

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
The `-f` or `--fix-broken` flag instructs APT to fix broken dependencies by downloading and configuring missing packages required to complete interrupted installations.
</details>

---

### Q2. Which command shows which installed package owns the file `/usr/bin/curl` on an Ubuntu system?
- [ ] A) `apt-cache search /usr/bin/curl`
- [ ] B) `dpkg -S /usr/bin/curl`
- [ ] C) `dpkg -L /usr/bin/curl`
- [ ] D) `apt-get source /usr/bin/curl`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
`dpkg -S <path>` (search) queries the local DPKG database to find which package owns a specific file on disk. `dpkg -L <pkg>` lists files installed by a package.
</details>

---

### Q3. On a RHEL/Rocky Linux server, which command queries an uninstalled remote package to see which files it provides?
- [ ] A) `rpm -ql <package>`
- [ ] B) `dnf repoquery -l <package>`
- [ ] C) `dnf provides */*`
- [ ] D) `rpm -qf <package>`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
`dnf repoquery -l <package>` queries remote repositories without downloading or installing the package. `rpm -ql` only works on locally installed packages or local `.rpm` files with `-qpl`.
</details>

---

### Q4. What is the primary purpose of `--no-install-recommends` when running `apt-get install` in a Docker container?
- [ ] A) It disables network access during installation
- [ ] B) It prevents the installation of non-essential recommended packages, reducing layer size
- [ ] C) It disables installation confirmation prompts
- [ ] D) It skips security advisory checks

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
Debian/Ubuntu packages declare `Depends`, `Recommends`, and `Suggests`. By default, APT installs `Recommends`. Using `--no-install-recommends` ensures only hard `Depends` are installed, significantly shrinking container footprint.
</details>

---

### Q5. What happens if you delete `/var/lib/dpkg/lock-frontend` while an active `unattended-upgrades` process is modifying packages?
- [ ] A) The system safely reboots
- [ ] B) The upgrade finishes 2x faster
- [ ] C) DPKG database corruption may occur due to simultaneous file write collisions
- [ ] D) APT automatically cancels the transaction

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: C</b><br>
Removing active lock files allows competing processes to access `/var/lib/dpkg/status` simultaneously, leading to half-written status files, corrupted package databases, and broken system states.
</details>

---

### Q6. Which Alpine Linux command installs packages without writing repository index files to the local disk cache?
- [ ] A) `apk add --no-cache <package>`
- [ ] B) `apk add --clean <package>`
- [ ] C) `apk install --purge <package>`
- [ ] D) `apk add --ephemeral <package>`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: A</b><br>
`apk add --no-cache` fetches package index files in memory, downloads and installs packages, and immediately discards the indexes, avoiding unnecessary disk overhead.
</details>

---

### Q7. On RHEL 9, which command will reverse transaction number 42 from the package manager history?
- [ ] A) `rpm --rollback 42`
- [ ] B) `dnf history rollback 42`
- [ ] C) `dnf history undo 42`
- [ ] D) `yum undo-transaction 42`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: C</b><br>
`dnf history undo 42` specifically undoes the operations performed in transaction 42. Note: `dnf history rollback 42` undoes *all* transactions that occurred after transaction 42 back to that point.
</details>

---

### Q8. Which command checks whether an installed RPM package's files have been modified or tampered with since installation?
- [ ] A) `rpm -V <package>`
- [ ] B) `rpm -qa <package>`
- [ ] C) `rpm -q --verify-md5 <package>`
- [ ] D) `dnf check-integrity <package>`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: A</b><br>
`rpm -V` (verify) compares file attributes, permissions, size, MD5/SHA256 checksums, and modification times against metadata stored when the package was installed.
</details>

---

### Q9. What does `dpkg -P <package>` do that `dpkg -r <package>` does not?
- [ ] A) Removes package binaries and all configuration files (Purge)
- [ ] B) Reinstalls previous package dependencies
- [ ] C) Prints package compilation logs
- [ ] D) Pins the package version

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: A</b><br>
`-r` (`--remove`) removes binaries and libraries but leaves configuration files intact in `/etc/`. `-P` (`--purge`) removes both the application files and all associated configuration files.
</details>

---

### Q10. Why is software compiled from source code conventionally installed under `/usr/local` rather than `/usr`?
- [ ] A) `/usr` is mounted as read-only on all Linux distributions
- [ ] B) To prevent custom builds from overwriting OS vendor packages managed by APT/DNF
- [ ] C) Because compilers cannot write files to `/usr`
- [ ] D) `/usr/local` has execution privileges disabled for security

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
According to the Filesystem Hierarchy Standard (FHS), `/usr` is reserved for vendor-managed packages (APT/DNF/RPM). Custom local installations belong in `/usr/local` or `/opt` to prevent collisions.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
