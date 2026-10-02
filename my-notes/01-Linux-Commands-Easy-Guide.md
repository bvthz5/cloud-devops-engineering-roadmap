# 🐧 Linux Commands — Simple Kids-Mind Easy Guide

> **Style:** Short 1-line definitions, advantages, and exact screen details you get when you type the command. Easy to memorize!

---

## 📁 1. Files & Folder Navigation Commands

### `pwd` (Print Working Directory)
- **Definition:** Tells you the exact folder you are currently sitting in.
- **Advantage:** You never get lost in Linux folders.
- **What details you get:** The full folder path starting from the root (e.g., `/home/ubuntu/projects`).
- **Example:** `pwd`

---

### `ls` (List Files)
- **Definition:** Shows the names of files and folders inside the current directory.
- **Advantage:** Quickly see what is inside a folder.
- **What details you get:** A list of file and folder names only (colored by type).
- **Example:** `ls`

### `ls -l` (Long Listing)
- **Definition:** Shows files with detailed information like size, owner, and date.
- **Advantage:** See permissions, file owner, exact size, and when it was edited.
- **What details you get:** 
  1. Permissions (e.g., `-rw-r--r--`)
  2. Hard link count
  3. Owner name (e.g., `root` or `ubuntu`)
  4. Group name
  5. File size in bytes
  6. Date & Time modified
  7. File/Folder name
- **Example:** `ls -l /var/log`

### `ls -a` (List All, Including Hidden Files)
- **Definition:** Shows all files including hidden files that start with a dot (`.`).
- **Advantage:** Find hidden configuration files like `.bashrc` or `.env`.
- **What details you get:** Standard file names plus `.` (current folder), `..` (parent folder), and files like `.git`, `.gitignore`.
- **Example:** `ls -a ~`

### `ls -la` / `ls -al` (Long List + Hidden Files)
- **Definition:** Combines `-l` and `-a` to show complete detailed information for every single file including hidden ones.
- **Advantage:** The most popular command used by DevOps engineers to inspect everything in a directory.
- **What details you get:** Full permissions, ownership, file size, timestamp, and hidden dotfiles together.
- **Example:** `ls -la`

### `ls -lh` (Human-Readable Sizes)
- **Definition:** Displays file sizes in friendly units (K for Kilobytes, M for Megabytes, G for Gigabytes) instead of raw bytes.
- **Advantage:** You instantly know if a file is 5MB or 5GB without counting digits.
- **What details you get:** Same as `ls -l`, but column 5 shows `4.2K`, `150M`, `2.1G`.
- **Example:** `ls -lh /var/log`

### `ls -lt` (Sort by Time / Newest First)
- **Definition:** Lists files sorted by the last time they were modified, newest at the top.
- **Advantage:** Instantly see the newest log files or recent edits.
- **What details you get:** Detailed file listing ordered from newest to oldest.
- **Example:** `ls -lt`

### `ls -R` (Recursive Listing)
- **Definition:** Lists files in the current folder, plus files inside all subfolders.
- **Advantage:** Explore an entire folder tree at once.
- **What details you get:** Folder name header followed by its contents, repeated for all nested folders.
- **Example:** `ls -R my-app/`

---

### `cd` (Change Directory)
- **Definition:** Moves your terminal from one folder into another folder.
- **Advantage:** Travel anywhere in the Linux filesystem.
- **What details you get:** No output on success; prompt updates with new path.
- **Example:** `cd /var/log`

### `cd ..` (Go Up One Folder)
- **Definition:** Moves you up into the parent folder that holds your current folder.
- **Advantage:** Quickly step back out of a folder.
- **What details you get:** Prompt moves up one level (e.g., from `/var/log` to `/var`).
- **Example:** `cd ..`

### `cd ~` or `cd` (Go Home)
- **Definition:** Instantly jumps to your user's personal home folder.
- **Advantage:** Quick escape back to your safe personal home directory.
- **What details you get:** Prompt shows `~` (e.g., `/home/ubuntu`).
- **Example:** `cd ~`

### `cd -` (Go to Previous Folder)
- **Definition:** Jumps back to the folder you were in right before your last `cd` command.
- **Advantage:** Like the "back" button on a TV remote; toggle between two folders.
- **What details you get:** Prints the path of the previous folder and moves you there.
- **Example:** `cd -`

---

### `mkdir` (Make Directory)
- **Definition:** Creates a new empty folder.
- **Advantage:** Organize your project files into neat folders.
- **What details you get:** Creates the folder silently.
- **Example:** `mkdir my-project`

### `mkdir -p` (Make Parent Directories as Needed)
- **Definition:** Creates nested folders all at once without giving an error if parent folders don't exist.
- **Advantage:** Create deep folder paths in a single command.
- **What details you get:** Creates all missing parent and child folders silently.
- **Example:** `mkdir -p app/src/components/ui`

---

### `touch` (Create Empty File or Update Timestamp)
- **Definition:** Creates a new empty file instantly if it doesn't exist, or updates its modified date if it does.
- **Advantage:** Super fast way to create empty files like `.env` or `app.py`.
- **What details you get:** Creates file with 0 bytes size.
- **Example:** `touch Dockerfile index.js`

---

### `cp` (Copy File)
- **Definition:** Makes a duplicate copy of a file in another location.
- **Advantage:** Back up important config files before editing.
- **What details you get:** File is duplicated to the destination.
- **Example:** `cp nginx.conf nginx.conf.bak`

### `cp -r` (Copy Folder Recursively)
- **Definition:** Copies an entire folder and all files and subfolders inside it.
- **Advantage:** Duplicate complete code folders safely.
- **What details you get:** All files and child directories are duplicated to new folder.
- **Example:** `cp -r /app /app_backup`

---

### `mv` (Move or Rename)
- **Definition:** Moves a file to a new folder, OR renames a file if kept in the same folder.
- **Advantage:** Clean way to rename files and relocate files without copying.
- **What details you get:** File disappears from old location/name and appears at new location/name.
- **Example:** `mv old.txt new.txt` or `mv app.log /var/log/`

---

### `rm` (Remove / Delete File)
- **Definition:** Permanently deletes a file from the disk.
- **Advantage:** Frees up disk space.
- **What details you get:** Deletes file without moving to a trash bin (permanent!).
- **Example:** `rm test.txt`

### `rm -r` (Remove Folder Recursively)
- **Definition:** Deletes a folder and all files/folders inside it.
- **Advantage:** Removes entire directory trees.
- **Example:** `rm -r old-build/`

### `rm -rf` (Force Remove Recursively — Be Careful!)
- **Definition:** Deletes a folder and everything inside forcibly without asking for confirmation, even if read-only.
- **Advantage:** Deletes stubborn folders like `node_modules` instantly.
- **Caution:** Never run `rm -rf /` or `rm -rf /*` (deletes entire operating system!).
- **Example:** `rm -rf node_modules/`

---

## 🔗 2. Linux Links & Inodes (`ln`, `ln -s`)

### What is an Inode?
- **1-Line Definition:** A unique serial number on your disk that stores a file's metadata (file size, owner, permissions, and disk blocks), but not its file name.
- **What details you get on screen (`ls -i`):** Prints the numeric Inode number before the filename (e.g., `1441824 my-app.js`).

---

### `ln` (Hard Link)
- **1-Line Definition:** Creates a direct twin file name pointing to the exact same physical Inode and disk blocks as the original file.
- **Advantage:** If someone deletes or renames the original file, your hard link still opens and preserves the data completely!
- **Limitation:** Cannot link directories, and cannot span across two different disk drives or partitions.
- **What details you get when you type it:** Runs silently on success.
- **What details appear on `ls -l`:** The link count (2nd column) increases from `1` to `2`.
- **Example:** `ln database.conf database.conf.bak`

---

### `ln -s` (Soft Link / Symbolic Link / Symlink)
- **1-Line Definition:** A lightweight shortcut file that simply stores the text path pointing to another file or directory (just like a Windows desktop shortcut).
- **Advantage:** Can link entire folders (`ln -s /var/log/nginx ./logs`) and can link across different disks, partitions, and network drives.
- **Limitation:** If the original file is deleted or moved, the soft link breaks (turns into a red "dangling link").
- **What details you get when you type it:** Runs silently on success.
- **What details appear on `ls -l`:**
  - File type starts with `l` (e.g., `lrwxrwxrwx 1 ubuntu ubuntu ...`)
  - Displays a visual arrow pointing to the destination: `app-shortcut -> /opt/apps/production/server.js`
- **Example (File):** `ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/app.conf`
- **Example (Directory):** `ln -s /var/log/my-app ./app-logs`

---

### `readlink -f <link>` (Resolve Real Path)
- **1-Line Definition:** Follows a symlink chain and prints the true, physical destination path on disk.
- **Advantage:** Easily find the real file behind nested shortcut links.
- **What details you get:** Prints the absolute real path (e.g., `/opt/apps/production/server.js`).
- **Example:** `readlink -f app-shortcut`

---

### `unlink <link>` (Remove a Link)
- **1-Line Definition:** Deletes the shortcut link cleanly without touching or damaging the original target file.
- **Advantage:** Safe way to remove symbolic links without accidentally deleting real directories.
- **Example:** `unlink ./app-logs`

---

## 📖 3. Viewing & Searching File Contents

### `cat` (Concatenate & Print)
- **Definition:** Prints the entire contents of a file directly onto the terminal screen.
- **Advantage:** Fast way to read small configuration files.
- **What details you get:** Raw text lines printed to screen from start to finish.
- **Example:** `cat /etc/os-release`

### `cat -n` (Print with Line Numbers)
- **Definition:** Prints file content with line numbers on the left.
- **Advantage:** Great for finding code or config errors referencing line numbers.
- **What details you get:** `1  server {`, `2    listen 80;`, etc.
- **Example:** `cat -n /etc/nginx/nginx.conf`

---

### `head` (View Top Lines)
- **Definition:** Shows the first 10 lines of a file.
- **Advantage:** Preview the start of large files without loading the whole file.
- **What details you get:** Top 10 lines of text.
- **Example:** `head /var/log/syslog`

### `head -n 25` (View Top N Lines)
- **Definition:** Shows the first N lines (e.g., first 25 lines) of a file.
- **Example:** `head -n 25 app.py`

---

### `tail` (View Bottom Lines)
- **Definition:** Shows the last 10 lines of a file.
- **Advantage:** Quickly check the latest error or output at the end of a log file.
- **What details you get:** Bottom 10 lines of text.
- **Example:** `tail /var/log/nginx/error.log`

### `tail -n 50` (View Bottom N Lines)
- **Definition:** Shows the last N lines (e.g., last 50 lines) of a file.
- **Example:** `tail -n 50 /var/log/auth.log`

### `tail -f` (Follow Live Log Stream — DevOps Super Tool!)
- **Definition:** Keeps the file open on screen and prints new lines in real-time as they are written.
- **Advantage:** Watch live web requests, server crashes, and errors as they happen.
- **What details you get:** Active scrolling live log output until you press `Ctrl+C`.
- **Example:** `tail -f /var/log/nginx/access.log`

---

### `less` (Paginated Interactive Viewer)
- **Definition:** Opens files in a clean, scrollable window (press `q` to quit, `/` to search).
- **Advantage:** View gigabyte-sized files without consuming RAM or freezing the terminal.
- **What details you get:** Interactive screen view with arrow key scrolling.
- **Example:** `less /var/log/messages`

---

### `grep` (Search Text inside Files)
- **Definition:** Searches for a specific word or pattern inside a file and prints matching lines.
- **Advantage:** Find errors or keywords inside huge log files in seconds.
- **What details you get:** Only lines that contain the search word.
- **Example:** `grep "ERROR" app.log`

### `grep -i` (Case-Insensitive Search)
- **Definition:** Searches ignoring upper/lower case (matches "error", "Error", "ERROR").
- **Example:** `grep -i "failed" /var/log/auth.log`

### `grep -r` (Recursive Search in All Files)
- **Definition:** Searches for text inside all files in a folder and subfolders.
- **Advantage:** Find which file contains a variable or database password.
- **What details you get:** File path + matching line content.
- **Example:** `grep -r "DB_PASSWORD" /etc/`

### `grep -v` (Invert Match / Exclude)
- **Definition:** Prints all lines that DO NOT contain the word.
- **Advantage:** Filter out noise (e.g., remove comment lines starting with `#`).
- **Example:** `grep -v "^#" /etc/nginx/nginx.conf`

### `grep -n` (Show Line Numbers)
- **Definition:** Prints matching lines along with their exact line number in the file.
- **Example:** `grep -n "timeout" my-config.yaml`

---

### `find` (Search for Files by Name or Type)
- **Definition:** Searches the filesystem to locate files and folders matching your search criteria.
- **Advantage:** Find lost files anywhere on the disk.
- **What details you get:** Full file paths of matching files.
- **Example:** `find /var/log -name "*.log"` (Find all .log files in /var/log)
- **Example:** `find . -type d -name "node_modules"` (Find node_modules folders)

---

## 🔒 4. Permissions & User Control Commands

### `chmod` (Change File Permissions)
- **Definition:** Changes who can Read (`r`), Write (`w`), and Execute (`x`) a file.
- **Advantage:** Secure files so unauthorized users cannot read or delete them.
- **What details you get:** Permission bits updated on the file.
- **Quick Number Rule:**
  - `7` = Read (4) + Write (2) + Execute (1) = Full Control
  - `6` = Read (4) + Write (2) = Read & Write
  - `5` = Read (4) + Execute (1) = Read & Run
  - `4` = Read (4) = Read Only
- **Common Examples:**
  - `chmod +x run.sh` (Make script executable so you can run `./run.sh`)
  - `chmod 600 id_rsa` (Private SSH key: only owner can read/write, blocks anyone else)
  - `chmod 644 file.txt` (Owner can read/write, everyone else read-only)
  - `chmod 755 script.sh` (Owner full control, others can read & execute)

---

### `chown` (Change File Owner & Group)
- **Definition:** Transfers ownership of a file or folder to another user or group.
- **Advantage:** Fixes "Permission denied" errors when applications need to write to a folder.
- **What details you get:** File ownership transferred to new user:group.
- **Example:** `chown ubuntu:ubuntu app.py`
- **Example:** `chown -R www-data:www-data /var/www/html` (Change owner for entire folder)

---

### `sudo` (SuperUser DO — Run as Administrator)
- **Definition:** Runs a command with root (system administrator) privileges.
- **Advantage:** Perform administrative tasks safely without logging in as root.
- **What details you get:** Prompts for your password and executes the privileged command.
- **Example:** `sudo apt update`

---

## 🖥️ 5. System, CPU, Memory & Disk Inspection Commands

### `whoami` (Who Am I)
- **Definition:** Tells you the username of the account you are currently logged in as.
- **What details you get:** Single word username (e.g., `ubuntu` or `root`).
- **Example:** `whoami`

### `uname -a` (System & Kernel Information)
- **Definition:** Prints full details about the Linux operating system, kernel version, and CPU architecture.
- **What details you get:** Linux OS name, hostname, kernel release (e.g., `5.15.0-101-generic`), build date, and architecture (`x86_64` or `aarch64`).
- **Example:** `uname -a`

### `uptime` (System Uptime & Load)
- **Definition:** Shows how long the server has been running without a reboot and average CPU load.
- **What details you get:** Current system time, days/hours up, active users count, and 1, 5, 15-minute load averages.
- **Example:** `uptime`

---

### `df -h` (Disk Free / Disk Usage in Human Units)
- **Definition:** Shows how full your hard drives and partitions are.
- **Advantage:** Immediately see if your disk is 99% full before servers crash.
- **What details you get:** 
  1. Filesystem name (`/dev/sda1`)
  2. Total Size (e.g., `50G`)
  3. Used Space (`15G`)
  4. Available Space (`35G`)
  5. Use Percentage (`30%`)
  6. Mounted On folder path (`/`)
- **Example:** `df -h`

### `df -i` (Inode Usage)
- **Definition:** Shows how many file index numbers (inodes) are free.
- **Advantage:** Discovers "No space left on device" crashes caused by millions of tiny files even when disk has free gigabytes!
- **What details you get:** Inodes total, used, free, and IUse% per partition.
- **Example:** `df -i`

---

### `du -sh` (Disk Usage Summary of a Folder)
- **Definition:** Calculates the exact total disk space taken up by a specific file or folder.
- **Advantage:** Find out exactly how many gigabytes a directory consumes.
- **What details you get:** Total size + folder name (e.g., `14G /var/log`).
- **Example:** `du -sh /var/log`
- **Example:** `du -sh *` (Shows size of every folder in current directory)

---

### `free -h` (Memory / RAM Status in Human Units)
- **Definition:** Displays total, used, and free RAM and Swap space.
- **Advantage:** Instantly check if server is running out of memory.
- **What details you get:** 
  - `Mem:` Total RAM, Used RAM, Free RAM, Shared, Buff/Cache, Available RAM (e.g., `16G total, 4G used, 10G available`).
  - `Swap:` Total swap disk space and used swap.
- **Example:** `free -h`

---

## ⚡ 6. Process Management & System Services

### `ps aux` (Process Snapshot)
- **Definition:** Lists every single running process on the entire machine.
- **Advantage:** Inspect what programs are running, who started them, and how much CPU/RAM they use.
- **What details you get:** 
  1. `USER` (Owner)
  2. `PID` (Process ID number — needed to kill it!)
  3. `%CPU` (CPU usage)
  4. `%MEM` (RAM usage)
  5. `VSZ / RSS` (Memory size)
  6. `STAT` (Status: R=Running, S=Sleeping, D=Disk Wait, Z=Zombie)
  7. `START / TIME` (Run duration)
  8. `COMMAND` (The exact program and arguments running)
- **Example:** `ps aux | grep nginx`

---

### `top` / `htop` (Real-Time Task Manager)
- **Definition:** Live interactive dashboard showing real-time CPU, RAM, and top resource-hogging processes.
- **Advantage:** See what process is spiking CPU to 100% right now.
- **What details you get:** Live updating list of processes sorted by CPU usage; press `q` to exit.
- **Example:** `top` (standard) or `htop` (colorful, modern version)

---

### `kill <PID>` (Graceful Process Termination)
- **Definition:** Sends a polite request (`SIGTERM / signal 15`) asking a process to save its work and close cleanly.
- **Example:** `kill 1234`

### `kill -9 <PID>` (Forced Immediate Kill — Emergency Stop)
- **Definition:** Sends `SIGKILL (signal 9)` which the Linux kernel executes immediately; the process cannot ignore it.
- **Advantage:** Instantly kills frozen, runaway, or unresponsive processes.
- **Example:** `kill -9 1234`

---

### `systemctl` (System Service Controller)
- **Definition:** The standard command used to start, stop, restart, and inspect background services (daemons).
- **Core Commands:**
  - `systemctl status nginx` (Shows if service is active/running, PID, recent logs, and uptime)
  - `systemctl start nginx` (Starts the service)
  - `systemctl stop nginx` (Stops the service)
  - `systemctl restart nginx` (Restarts the service — drops connections briefly)
  - `systemctl reload nginx` (Hot-reloads config files without dropping connections!)
  - `systemctl enable nginx` (Makes service start automatically on machine reboot)

---

## 🌐 7. Network Inspection Commands (IP, Sockets & Ports)

### `hostname -I` (Show Host IP Addresses)
- **Definition:** Prints all network IP addresses assigned to this machine on a single line.
- **Advantage:** Fastest way to find your machine's private IP.
- **What details you get:** Space-separated IP addresses (e.g., `192.168.1.50 10.0.0.15`).
- **Example:** `hostname -I`

---

### `ip addr` (or `ip a`) (Detailed IP Addresses & Network Interfaces)
- **Definition:** Shows all network adapters (Ethernet, Wi-Fi, Loopback, Docker bridges) and their assigned IPv4/IPv6 addresses.
- **Advantage:** Modern replacement for legacy `ifconfig`.
- **What details you get:** 
  1. Interface number & name (e.g., `1: lo:`, `2: eth0:`, `3: docker0:`)
  2. Interface State (`UP` or `DOWN`)
  3. MAC Address (`link/ether 02:42:ac:11:00:02`)
  4. IPv4 Address & Subnet CIDR (`inet 192.168.1.15/24`)
  5. Broadcast address (`brd 192.168.1.255`)
  6. IPv6 Address (`inet6 fe80::...`)
- **Example:** `ip addr`

---

### `ip link` (Network Interfaces & Physical Hardware State)
- **Definition:** Displays only network layer 2 interfaces, their hardware MAC addresses, MTU, and link state.
- **Advantage:** Check if your network cable is plugged in or interface is UP without IP clutter.
- **What details you get:** Interface name, flags (`UP,BROADCAST,RUNNING`), MTU (e.g., `mtu 1500`), and MAC address.
- **Example:** `ip link`

---

### `ip route` (Routing Table & Default Gateway)
- **Definition:** Displays the kernel network routing table (how packets find the internet).
- **Advantage:** Find your Default Gateway router IP and local subnet routes.
- **What details you get:** 
  - `default via 192.168.1.1 dev eth0` (Default router IP and exit network card)
  - Subnet routes (e.g., `192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.15`)
- **Example:** `ip route`

---

### `ping <HOST>` (Network Connectivity & Latency Test)
- **Definition:** Sends ICMP Echo requests to a remote IP/domain to test if it's reachable and measure round-trip time.
- **Advantage:** First step to diagnose "Can my server reach Google / database?".
- **What details you get:** Packet sequence number, TTL, round-trip time in milliseconds (e.g., `time=14.2 ms`).
- **Example:** `ping -c 4 8.8.8.8` (`-c 4` stops automatically after 4 packets!)

---

### `ss -tulpn` (Active Sockets & Listening Ports)
- **Definition:** Lists all listening network ports and the exact process name and PID using them.
- **Advantage:** Modern, high-speed replacement for legacy `netstat`.
- **Flags Breakdown:**
  - `-t`: TCP sockets
  - `-u`: UDP sockets
  - `-l`: Only LISTENING ports (servers waiting for connections)
  - `-p`: Show Process Name and PID
  - `-n`: Numeric IP and Port numbers (shows `:80` instead of `http`)
- **What details you get:** Protocol (`tcp`), State (`LISTEN`), Local IP & Port (`0.0.0.0:80`), and Process info (`users:(("nginx",pid=1200,fd=6))`).
- **Example:** `ss -tulpn`

---

### `curl` (Command Line HTTP Client)
- **Definition:** Transmits HTTP/HTTPS web requests and prints the response.
- **Advantage:** Test APIs, download web pages, and verify web servers from the command line.
- **Flags Breakdown:**
  - `curl http://example.com` (Prints raw HTML/JSON body)
  - `curl -I http://example.com` (Fetch HTTP Headers only — shows status code `200 OK`, `301`, `404`)
  - `curl -v http://example.com` (Verbose mode — shows DNS lookup, IP connection, TLS handshake, and headers)
  - `curl -O https://example.com/file.tar.gz` (Downloads file and saves with original filename)

---

---

## 📦 8. Archives & File Compression

### `tar` (Tape Archive — Compression & Packaging)
- **1-Line Definition:** Bundles multiple files and folders into a single archive file, optionally compressed with gzip.
- **Advantage:** Preserves file permissions and directory structure when creating backups.
- **Flags Breakdown:**
  - `tar -czvf backup.tar.gz /var/log` (Create compressed archive)
    - `-c`: Create new archive
    - `-z`: Compress with gzip (`.tar.gz`)
    - `-v`: Verbose (lists each file being added on screen)
    - `-f`: Output file name
  - `tar -xzvf backup.tar.gz -C /opt/` (Extract archive to destination)
    - `-x`: Extract files
    - `-C`: Target directory to unpack into
- **What details you get:** Prints each file path as it is packed or extracted, followed by final archive file on disk.

### `gzip` & `gunzip` (Single File Compression)
- **1-Line Definition:** Compresses a single large file to save disk space (`gzip`), or decompresses it back (`gunzip`).
- **Advantage:** Reduces log files by 80–90% size.
- **Example:** `gzip access.log` (creates `access.log.gz` and removes uncompressed file).

---

## ⚙️ 9. Package Management & Environment Variables

### Package Management (`apt` for Ubuntu/Debian, `dnf`/`yum` for RHEL/CentOS)
- **1-Line Definition:** The system app store that downloads, installs, updates, and removes software packages.
- **Commands:**
  - `sudo apt update`: Refreshes package lists and version numbers from remote repositories.
  - `sudo apt upgrade -y`: Installs newest security patches and versions for all installed software.
  - `sudo apt install -y nginx`: Downloads and installs a software package automatically.
  - `sudo apt remove -y nginx`: Uninstalls the software binary.
  - `sudo apt autoremove -y`: Cleans up orphaned dependencies that are no longer needed.

### Environment Variables (`export`, `env`, `$PATH`)
- **1-Line Definition:** System-wide key-value variables that applications and shells read to discover configuration paths and settings.
- **Commands:**
  - `env`: Prints all currently active environment variables on screen.
  - `echo $PATH`: Shows the list of directories Linux searches to find executable commands.
  - `export DB_URL="postgres://localhost:5432"`: Sets an environment variable in the current shell session.
  - Permanent setup: Add `export KEY=VALUE` into `~/.bashrc` (user) or `/etc/environment` (system-wide).

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Notes Index](./00-Index.md) | [Index](../README.md) | [Windows & PowerShell Easy Guide →](./02-Windows-PowerShell-CMD-Easy-Guide.md) |
