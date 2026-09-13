# 18 - Multiple Choice Questions (MCQ): Linux Folder Structure

Test your knowledge on the Linux Filesystem Hierarchy Standard (FHS).

---

### Q1. Which directory contains system-wide plain-text configuration files in Linux?
- A) `/usr/bin`
- B) `/etc`
- C) `/var/lib`
- D) `/proc`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) `/etc`**

**Explanation:**
`/etc` is reserved for system-wide configuration files (e.g., `/etc/fstab`, `/etc/passwd`, `/etc/nginx/nginx.conf`).
</details>

---

### Q2. Where are device nodes (like block drives `/dev/sda` or character interfaces `/dev/tty`) located?
- A) `/sys`
- B) `/proc`
- C) `/dev`
- D) `/mnt`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **C) `/dev`**

**Explanation:**
`/dev` (devtmpfs) contains device nodes representing physical and pseudo hardware devices.
</details>

---

### Q3. What happens to files stored in `/tmp` when the system reboots on modern Linux distros?
- A) They are permanently preserved in `/var/tmp`.
- B) They are automatically cleared (since `/tmp` is backed by RAM/`tmpfs` or cleared by boot scripts).
- C) They are moved to `/home/root`.
- D) An error is logged in `/var/log`.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) They are automatically cleared.**

**Explanation:**
`/tmp` is intended for temporary file storage and is wiped automatically during boot or by systemd cleanup timers.
</details>

---

### Q4. Which directory is designated for third-party, self-contained optional software packages?
- A) `/opt`
- B) `/srv`
- C) `/usr/local`
- D) `/mnt`

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **A) `/opt`**

**Explanation:**
`/opt` holds optional third-party software packages (e.g., `/opt/google/chrome`, `/opt/gitlab`).
</details>

---

### Q5. Under modern "usr-merge" Linux distributions, what is `/bin`?
- A) A separate physical partition for administrative commands.
- B) A symbolic link pointing to `/usr/bin`.
- C) A virtual pseudo-filesystem.
- D) A subfolder inside `/home`.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) A symbolic link pointing to `/usr/bin`.**

**Explanation:**
Under usr-merge, legacy binary directories (`/bin`, `/sbin`, `/lib`) are symbolic links pointing to `/usr/bin`, `/usr/sbin`, and `/usr/lib`.
</details>
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [17 - Hands On Practice](./17-Hands-On-Practice.md) | [README](./README.md) | [19 - Quick Revision](./19-Quick-Revision.md) |
