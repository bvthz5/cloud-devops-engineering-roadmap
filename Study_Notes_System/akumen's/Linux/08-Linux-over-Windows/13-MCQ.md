# 13 - Multiple Choice Questions (MCQ): Linux over Windows

Test your knowledge on why Linux is chosen over Windows for cloud and server infrastructure.

---

### Q1. What open-source license governs the Linux kernel?
- A) MIT License
- B) Apache 2.0 License
- C) GNU General Public License (GPL)
- D) Proprietary Commercial License

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **C) GNU General Public License (GPL)**

**Explanation:**
The Linux kernel is licensed under GNU GPL v2, granting rights to use, modify, and redistribute code freely without OS software licensing fees.
</details>

---

### Q2. Which two native Linux kernel features form the underlying foundation of Docker containers?
- A) Win32 API & Registry
- B) Control Groups (`cgroups`) & Namespaces
- C) NTFS permissions & Hyper-V
- D) Systemd & Journald

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) Control Groups (`cgroups`) & Namespaces**

**Explanation:**
`cgroups` enforce CPU/memory resource boundaries, while `namespaces` provide isolated process, network, and filesystem views for native containers.
</details>

---

### Q3. Why do headless Linux servers consume significantly less RAM than Windows Server?
- A) Linux uses 16-bit architecture.
- B) Headless Linux operates without loading a graphical desktop shell (X11/Wayland/GNOME).
- C) Windows compresses RAM pages by default.
- D) Linux disables networking by default.

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) Headless Linux operates without loading a graphical desktop shell.**

**Explanation:**
Headless Linux servers run entirely via CLI/SSH without graphical desktop overhead, reserving RAM for application workloads.
</details>

---

### Q4. Which line ending character sequence is standard in Linux files?
- A) CRLF (`\r\n`)
- B) LF (`\n`)
- C) CR (`\r`)
- D) EOF (`\0`)

<details>
<summary><b>View Answer & Explanation</b></summary>

**Correct Answer:** **B) LF (`\n`)**

**Explanation:**
Linux uses LF (`\n`), whereas Windows uses CRLF (`\r\n`). CRLF line endings in Linux scripts cause `bad interpreter` errors.
</details>

---

## ⬅️ Navigation
- Previous: [12 - Hands-On Practice](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/08-Linux-over-Windows/12-Hands-On-Practice.md)
- Next: [14 - Quick Revision Cheat Sheet](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/08-Linux-over-Windows/14-Quick-Revision.md)
