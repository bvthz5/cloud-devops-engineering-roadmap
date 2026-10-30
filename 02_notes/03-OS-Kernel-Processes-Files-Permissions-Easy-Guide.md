# 🐧 OS, Kernel, Processes, Files & Permissions (Kids-Mind Easy Guide)

> **Style:** Short 1-line definitions, architecture diagrams, step-by-step command lifecycles, and production permission troubleshooting.

---

## 📜 1. History of Operating Systems, Linux, & Unix: Who, When, & What Crisis Sparked the Invention?

| Technology / Milestone | Inventor(s) & Organization | Year | The Real Problem / Crisis That Triggered It | Kids-Mind Analogy |
| :--- | :--- | :--- | :--- | :--- |
| **Unix Operating System** | **Ken Thompson, Dennis Ritchie, Rudd Canaday, Doug McIlroy** (AT&T Bell Labs) | **1969** | **The Multics Failure**: Bell Labs pulled out of the massive Multics project because it was bloated, over-engineered, and slow. Thompson and Ritchie wanted a fast, elegant, interactive multi-user OS. They formulated the Unix philosophy: *"Write programs that do one thing and do it well, and work together using plain text."* | Building with small, modular Lego bricks instead of carving one giant heavy stone statue. |
| **C Programming Language** | **Dennis Ritchie** (Bell Labs) | **1972** | The original 1969 Unix was written in PDP-7 assembly language, making it tied to one single machine model. Ritchie invented C with pointers and low-level memory access so the entire Unix kernel could be rewritten portably in 1973 to run on *any* computer hardware. | Inventing a universal lingua franca so books don't have to be hand-rewritten in every town's dialect. |
| **The Shell & Pipes (`sh` / `|`)** | **Stephen Bourne** (`sh`, 1979) / **Doug McIlroy** (Pipes, 1973) | **1973 / 1979** | Computers required engineers to write monolithic programs for every task. Doug McIlroy invented the Pipe (`|`), and Stephen Bourne wrote the Bourne Shell (`sh`), allowing users to connect the output of one tool directly into the input of another tool like plumbing. | Connecting water pipes together so clean water flows directly from the filter to the tap. |
| **The GNU Project & Free Software** | **Richard Stallman** (MIT AI Lab / FSF) | **1983** | In the early 1980s, AT&T commercialized Unix and forced programmers to sign non-disclosure agreements, forbidding them from sharing or fixing code. Stallman launched the GNU Project (*"GNU's Not Unix"*) and GPL license to guarantee all humans the freedom to run, study, modify, and redistribute software. | Open-sourcing recipes so all bakers can share, improve, and bake bread without paying patent royalties. |
| **The Linux Kernel** | **Linus Torvalds** (University of Helsinki) | **1991** | By 1991, the GNU Project had built compilers, editors, and shells, but lacked a working kernel (GNU Hurd was delayed). Andrew Tanenbaum's MINIX had licensing restrictions preventing commercial use. 21-year-old Linus Torvalds wrote a free, monolithic 386 Unix kernel and released it to the world. | Providing the missing engine block for an already-assembled car chassis. |
| **The Bourne-Again Shell (`bash`)** | **Brian Fox & Chet Ramey** (GNU Project) | **1989** | The original Unix Bourne shell (`sh`) was proprietary and lacked interactive conveniences like command history, tab completion, and aliases. Fox created `bash` as the 100% free GNU replacement, now the default shell across Linux distributions. | An upgraded dashboard with power steering, GPS, and cruise control. |
| **`sudo` (SuperUser DO)** | **Bob Coggeshall & Cliff Spencer** (SUNY Buffalo) | **1980** | Multi-user university labs had student operators who needed to mount backup tapes or reset printer queues, but giving them the `root` password let them accidentally delete the entire disk (`rm -rf /`). Coggeshall created `sudo` to grant granular root privileges with an audit log. | Giving a hotel valet key that only starts the car and parks it, but cannot open the locked glove compartment or trunk. |
| **`systemd` (System & Service Manager)** | **Lennart Poettering & Kay Sievers** (Red Hat) | **2010** | Classic Unix SysVinit ran startup scripts sequentially one by one (`/etc/init.d/`), causing servers to take minutes to boot. It could not handle dynamic hotplugged hardware, network mounts, or isolate container process groups (cgroups). `systemd` introduced parallel socket-activated service startup. | An automated airport control tower directing 20 planes simultaneously instead of a single flag waver. |

### 🕰️ Visual Timeline: The Evolution of Unix & Linux

```text
 1969 ────── 1972 ────── 1979 ────── 1980 ────── 1983 ────── 1989 ────── 1991 ────── 2010 ───── Present
   │           │           │           │           │           │           │           │           │
 Unix       C Lang      Bourne       sudo        GNU        bash       Linux       systemd     Modern Cloud
Bell Labs   Dennis       Shell     Buffalo     Richard      Brian      Linus       Red Hat     Linux Server
Thompson    Ritchie     Pipes      Spencer    Stallman       Fox      Torvalds   Poettering    Dominance
```

---

## 🎛️ 2. What is an Operating System & The Kernel?

An **Operating System (OS)** is system software that manages hardware resources and provides a platform for applications to run.

```text
┌────────────────────────────────────────────────────────┐
│ USER SPACE (Applications run safely here)              │
│  - Web Browser     - Node.js API       - Nginx Server  │
└───────────────────────────┬────────────────────────────┘
                            │ System Calls (syscalls: open, read, fork)
                            ▼
┌────────────────────────────────────────────────────────┐
│ KERNEL SPACE (The privileged core of the OS)           │
│  - CPU Process Scheduler       - Virtual Memory & RAM  │
│  - VFS Filesystem (ext4, XFS)  - Network Stack (TCP/IP)│
│  - Hardware Device Drivers     - Security & cgroups    │
└───────────────────────────┬────────────────────────────┘
                            │ Hardware Control Signals
                            ▼
┌────────────────────────────────────────────────────────┐
│ PHYSICAL HARDWARE: CPU, RAM Chips, NVMe SSD, NIC       │
└────────────────────────────────────────────────────────┘
```

- **User Space:** Where your applications run with restricted permissions. If an app crashes here, the OS keeps running.
- **Kernel Space:** The privileged brain running with full hardware control. If the kernel crashes, the entire system crashes (**Kernel Panic** in Linux / **BSOD** in Windows).
- **System Call:** The official doorway. An application cannot directly touch the physical hard drive; it asks the kernel via a system call (`read()`, `write()`, `fork()`).

---

## 💻 3. Terminal vs. Shell vs. Command

```text
┌───────────────────────────┬───────────────────────────┬───────────────────────────┐
│ Terminal                  │ Shell (e.g. Bash, Zsh)    │ Command                   │
├───────────────────────────┼───────────────────────────┼───────────────────────────┤
│ The graphical window      │ The interpreter program   │ The specific instruction  │
│ where you type text.      │ that reads your input and │ given to the shell        │
│ Example: GNOME Terminal,  │ talks to the OS kernel.   │ (e.g., `mkdir lab`,       │
│ Windows Terminal, iTerm2. │ Example: `/bin/bash`      │ `pwd`, `ls -la`).         │
└───────────────────────────┴───────────────────────────┴───────────────────────────┘
```

### What happens when you type `mkdir devops-lab`?
1. You hit `Enter` in the terminal.
2. The shell parses the word `mkdir` and its argument `devops-lab`.
3. The shell searches directories in `$PATH` to locate the binary `/bin/mkdir`.
4. The shell asks the kernel to create a child process via `fork()` and `execve()`.
5. The kernel checks your user write permissions on the directory.
6. The filesystem creates the directory entry and control returns to your prompt.

---

## 📄 4. Configuration File Formats (YAML, JSON, XML, TOML)

DevOps tools configure applications and cloud infrastructure using human-readable text formats:

| Format | Primary DevOps Use | Strengths | Watch Out For! |
| :--- | :--- | :--- | :--- |
| **YAML** | Docker Compose, Kubernetes, GitHub Actions | Clean syntax, no curly braces, easy to read | **Indentation matters!** (Use spaces, never tabs). |
| **JSON** | APIs, `package.json`, AWS policy documents | Strict, universally supported by programming languages | No comments allowed; syntax errors if a comma is missing. |
| **XML** | Java Maven (`pom.xml`), older enterprise tools | Highly structured with schemas | Verbose, repetitive opening/closing tags (`<app></app>`). |
| **TOML** | Python (`pyproject.toml`), Rust (`Cargo.toml`) | Section-based headers, very clean for settings | Less common for complex nested trees. |

### YAML Indentation Example:
```yaml
app:
  name: shopping-api
  port: 3000
  environment: production
```

---

## ⚡ 5. Process vs. Thread vs. Service/Daemon

```text
┌────────────────────────────────────────────────────────┐
│ PROCESS (PID: 1420 - Independent Address Space)        │
│  ├── Code, Data, Open Files, Working Directory         │
│  │                                                     │
│  ├── Thread 1: Handles incoming HTTP client requests   │
│  ├── Thread 2: Writes database audit logs              │
│  └── Thread 3: Sends email notification in background  │
└────────────────────────────────────────────────────────┘
```

| Dimension | Program | Process | Thread | Service / Daemon |
| :--- | :--- | :--- | :--- | :--- |
| **What is it?** | Dead code sitting on disk | A living instance of a program running in memory | A lightweight execution unit inside a process | A background service managed by the OS |
| **Identification**| File path (`/bin/node`) | **PID** (Process ID, e.g. 1042) | Thread ID (TID) | Service unit name (`nginx.service`) |
| **Memory** | Consumes disk space | Own isolated virtual memory | Shares parent process memory | Runs in its own process memory |
| **Crash Impact** | Does not affect anything | Dies independently | Can crash entire process | Systemd can automatically restart it |

### Inspecting Processes in Linux:
```bash
# List all running processes with CPU & RAM usage
ps aux

# Find process ID of an app by name
pgrep -a nginx

# Display hierarchical process tree showing parent/child relationships
pstree -p
```

- **PID 1 (`systemd`):** The ancestor of all processes. It boots the system, starts daemons, and reaps orphaned child processes.
- **PPID (Parent Process ID):** The ID of the process that launched this process.
- **Zombie Process (`Z` state):** A dead process that has finished executing, but its parent has not yet read its exit code. Killing a zombie with `kill -9` fails because it is already dead! You must restart or signal its parent process.

---

## 🚦 6. Managing Background Services with `systemctl`

System services (daemons) run 24/7 in the background without needing a terminal open:

```bash
# Check if service is active and healthy
sudo systemctl status nginx

# Start the service right now
sudo systemctl start nginx

# Stop the service right now
sudo systemctl stop nginx

# Restart the service (drops connections briefly)
sudo systemctl restart nginx

# Reload configuration without dropping connections
sudo systemctl reload nginx

# Enable service to start automatically whenever the server boots
sudo systemctl enable nginx

# Inspect live service logs
sudo journalctl -u nginx -f
```

---

## 📂 7. Files, Directories & Paths

Think of Linux storage as a physical filing cabinet:
- **Filesystem:** The cabinet itself (e.g., `ext4`, `XFS`).
- **Directory:** A drawer or folder inside the cabinet.
- **File:** A sheet of paper containing data.

### Absolute vs. Relative Paths:

```text
Current Location: /home/binil/devops-lab
Target File:      /home/binil/devops-lab/linux/commands.txt

Absolute Path (Starts from root '/'):
  /home/binil/devops-lab/linux/commands.txt   <── Works from ANY directory!

Relative Path (Starts from current folder):
  linux/commands.txt                          <── Only works when you are in devops-lab!
```

- `/` = Filesystem root (top of the entire system).
- `.` = Current directory.
- `..` = Parent directory (one folder up).
- `~` = Your user's home folder (`/home/binil`).

---

## 🌐 8. Environment Variables & `$PATH`

An **Environment Variable** is a named key-value pair provided to a running process by the shell:

```bash
# Set a shell variable for current terminal only:
PORT=3000

# Export it so child processes and background apps inherit it:
export PORT=3000

# Print variable value:
echo "$PORT"
```

### Why `$PATH` is the Most Important Variable in Linux:
When you type `docker` or `ls`, Linux does not guess where the file is. It checks every folder listed in `$PATH` sequentially from left to right:
```bash
echo "$PATH"
# /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
> ⚠️ **Common DevOps Bug: `command not found`:** This means either the tool is not installed, or its installation folder is missing from the `$PATH` variable!

---

## 🛡️ 9. Linux Permissions, Ownership & `sudo`

Run `ls -l notes.txt`:

```text
-rw-r--r-- 1 binil devteam 120 Oct 10 10:00 notes.txt
┬───────── ┬ ───── ─────── ─── ──────────── ─────────
│          │ │     │       │   │            └─ File Name
│          │ │     │       │   └─ Last Modified Timestamp
│          │ │     │       └─ Size in Bytes
│          │ │     └─ Owning Group
│          │ └─ Owning User
│          └─ Hard Link Count
└─ Permissions: [Type][Owner: rw-][Group: r--][Others: r--]
```

### The 3 Permission Types:
- **Read (`r` = 4):** View file contents / list directory contents.
- **Write (`w` = 2):** Edit file / create or delete files inside directory.
- **Execute (`x` = 1):** Run file as program / enter (`cd`) and traverse directory.

### Calculating Permission Numbers:
Add the values for each of the three categories:

$$\mathbf{rwx} = 4 + 2 + 1 = \mathbf{7} \qquad \mathbf{rw-} = 4 + 2 + 0 = \mathbf{6} \qquad \mathbf{r-x} = 4 + 0 + 1 = \mathbf{5} \qquad \mathbf{r--} = 4 + 0 + 0 = \mathbf{4}$$

```text
┌──────────────────────┬───────┬───────┬────────┬─────────────────────────────┐
│ Command              │ Owner │ Group │ Others │ Typical Use Case            │
├──────────────────────┼───────┼───────┼────────┼─────────────────────────────┤
│ chmod 755 script.sh  │ rwx   │ r-x   │ r-x    │ Executable scripts / Folders│
│ chmod 644 config.txt │ rw-   │ r--   │ r--    │ Standard readable files     │
│ chmod 600 id_rsa     │ rw-   │ ---   │ ---    │ Private SSH keys & secrets  │
│ chmod 770 team_dir   │ rwx   │ rwx   │ ---    │ Shared developer folder     │
│ chmod 777 file.txt   │ rwx   │ rwx   │ rwx    │ ❌ DANGEROUS! Never use!    │
└──────────────────────┴───────┴───────┴────────┴─────────────────────────────┘
```

> ⚠️ **Golden Security Rule:** Never run `chmod 777` to fix permission errors! It allows any attacker or malicious script on the machine to modify, overwrite, or delete your files.

---

## 👥 10. Managing Users, Groups & Shared Team Folders

```bash
# 1. Create a development group
sudo groupadd devteam

# 2. Create a new developer user with home directory and bash shell
sudo useradd -m -s /bin/bash developer1

# 3. Add user to the devteam group (-a = append to existing groups!)
sudo usermod -aG devteam developer1

# 4. Create shared project directory
sudo mkdir -p /opt/devops-project

# 5. Change ownership to root:devteam
sudo chown root:devteam /opt/devops-project

# 6. Set SGID (Set Group ID) bit so all newly created files inherit devteam group
sudo chmod 2770 /opt/devops-project
```

- **SGID (`chmod 2770`):** The `2` sets the SGID bit (`drwxrws---`). Any developer who creates a file in `/opt/devops-project` will automatically have it owned by group `devteam`, allowing teammates to edit it!

---

## 🔍 11. Real-World Troubleshooting: `Permission denied`

When a CI/CD pipeline or Docker container fails with `Permission denied`, follow this systematic 4-step triage runbook:

```text
Step 1: Check Current Identity
  $ whoami && id
  Find the exact user and group running the operation.
       │
       ▼
Step 2: Inspect Target File Permissions & Ownership
  $ ls -ld /opt/myapp/data
  Does the user or their group have 'r' / 'w' / 'x' rights?
       │
       ▼
Step 3: Check Parent Directory Traversal
  $ namei -l /opt/myapp/data
  If ANY parent folder (e.g. /opt) lacks execute ('x') for the user,
  the user CANNOT enter or reach the file!
       │
       ▼
Step 4: Apply the Correct Fix
  - If service should own it: sudo chown -R appuser:appgroup /opt/myapp
  - If user needs access: sudo usermod -aG appgroup <user>
  - If script needs execution: chmod +x deploy.sh
```

---

## 🛠️ 12. Hands-On Practice Lab

Practice these commands on your Linux system:

```bash
# 1. Create workspace
mkdir -p ~/devops-lab/permissions && cd ~/devops-lab/permissions

# 2. Create sample file
echo "Hello DevOps" > test.txt

# 3. Set 640 permissions (Owner rw, Group r, Others none)
chmod 640 test.txt

# 4. Verify exact numeric mode and symbolic string
stat -c '%A %a %U %G %n' test.txt
# Output: -rw-r----- 640 binil binil test.txt

# 5. Start a background sleep process for 300 seconds
sleep 300 &

# 6. Find its PID
pgrep -a sleep

# 7. Terminate it safely using SIGTERM
kill $(pgrep sleep)
```

---

## 📝 13. Quick Revision Quiz

1. **What is the difference between a program and a process?**
   - Answer: A program is passive code stored on disk; a process is an active, running instance in memory with a PID.
2. **What does `chmod 755 file.sh` grant?**
   - Answer: Owner has read/write/execute (`rwx`), Group and Others have read/execute (`r-x`).
3. **What is the primary role of the `PATH` environment variable?**
   - Answer: Lists directories where the shell searches for executable commands.
4. **Why does killing a zombie process (`Z` state) with `kill -9` fail?**
   - Answer: Because a zombie is already dead; you must signal or restart its parent process.
5. **What does the SGID bit (`chmod 2770`) do on a shared folder?**
   - Answer: Causes newly created files inside the folder to automatically inherit the folder's group ownership.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Hardware, CPU, GPU, TPU & Units](./02-Computer-Foundations-Hardware-CPU-GPU-TPU-Binary-Easy-Guide.md) | [Index](../README.md) | [Linux Commands Easy Guide →](./04-Linux-Commands-Easy-Guide.md) |
