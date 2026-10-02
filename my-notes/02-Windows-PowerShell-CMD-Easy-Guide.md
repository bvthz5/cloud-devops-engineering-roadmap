# 🪟 Windows CMD & PowerShell Commands — Simple Kids-Mind Easy Guide

> **Style:** Short 1-line definitions, advantages, and exact screen details you get when you type the command. Easy to memorize!

---

## 🌐 1. Windows Network Inspection Commands

### `ipconfig` (Basic Network Summary)
- **Definition:** Shows the primary IP address, Subnet Mask, and Default Gateway router for all connected network cards.
- **Advantage:** Quickest way to check if your computer has an IP address from your Wi-Fi or Ethernet.
- **What details you get when you type it:**
  - Network Adapter Name (e.g., `Wireless LAN adapter Wi-Fi:`)
  - Connection-specific DNS Suffix (e.g., `lan` or `localdomain`)
  - `IPv4 Address`: Your computer's local network IP (e.g., `192.168.1.50`)
  - `Subnet Mask`: Network size identifier (e.g., `255.255.255.0`)
  - `Default Gateway`: Your home or office Wi-Fi router IP (e.g., `192.168.1.1`)
- **Example:** `ipconfig`

---

### `ipconfig /all` (Full Network Diagnostic Report — The Master Command!)
- **Definition:** Shows every single technical detail about your computer's networking hardware, IP addresses, DNS servers, and DHCP leases.
- **Advantage:** Discovers MAC hardware addresses, DNS server IPs, and whether DHCP is working.
- **What details you get when you type it:**
  1. `Host Name`: The computer name (e.g., `DESKTOP-DEV01`)
  2. `Physical Address`: The unique 12-digit hardware **MAC Address** of the Wi-Fi/Ethernet chip (e.g., `00-1A-2B-3C-4D-5E`)
  3. `DHCP Enabled`: `Yes` (dynamic IP from router) or `No` (static manual IP)
  4. `IPv4 Address` & `IPv6 Address`: Complete local IP addresses
  5. `Lease Obtained` & `Lease Expires`: Exact date and time your IP lease expires
  6. `Default Gateway`: Router IP address
  7. `DHCP Server`: The IP of the router/server that gave you this IP
  8. `DNS Servers`: The domain name resolvers your computer queries (e.g., `8.8.8.8` or `192.168.1.1`)
- **Example:** `ipconfig /all`

---

### `ipconfig /flushdns` (Clear DNS Cache)
- **Definition:** Erases your computer's temporary memory of domain names and their old IP addresses.
- **Advantage:** Fixes website loading errors immediately when a domain moves to a new server or IP.
- **What details you get when you type it:** `Successfully flushed the DNS Resolver Cache.`
- **Example:** `ipconfig /flushdns`

---

### `ipconfig /release` (Disconnect IP Lease)
- **Definition:** Drops and gives up your computer's current IP address back to the router.
- **Advantage:** First step in resetting a bugged or broken network connection.
- **What details you get when you type it:** All network adapters display blank `0.0.0.0` IP addresses.
- **Example:** `ipconfig /release`

---

### `ipconfig /renew` (Request Fresh IP Address)
- **Definition:** Asks your Wi-Fi router to assign your computer a brand-new IP address.
- **Advantage:** Reconnects you to the internet with a fresh IP address after running `/release`.
- **What details you get when you type it:** Pauses for 2 seconds, then prints your newly acquired IPv4 address and Gateway.
- **Example:** `ipconfig /renew`

---

### `tracert <HOST>` (Trace Route Hops in Windows)
- **Definition:** Shows every router and server hop your packets travel through to reach a website.
- **Advantage:** Discovers exactly which router or telecom cable is failing when a site is down.
- **What details you get when you type it:** Hop number (1, 2, 3...), latency in milliseconds across 3 attempts, and router IP address.
- **Example:** `tracert google.com`

---

### `nslookup <DOMAIN>` (Name Server Lookup)
- **Definition:** Asks your DNS server to translate a human domain name into its computer IP address.
- **Advantage:** Check if a website's DNS record has updated.
- **What details you get when you type it:**
  - `Server`: The DNS server answering (e.g., `dns.google`)
  - `Address`: The DNS server's IP (e.g., `8.8.8.8`)
  - `Name`: The requested domain (e.g., `github.com`)
  - `Address`: The target server's public IP address (e.g., `140.82.121.3`)
- **Example:** `nslookup github.com`

---

### `netstat -ano` (Active Sockets, Ports & Process IDs)
- **Definition:** Displays all active network connections, listening ports, and the Process ID (PID) owning each port.
- **Advantage:** Find out which application is hogging port 80, 443, 3306, or 8080.
- **What details you get when you type it:**
  - `Proto`: `TCP` or `UDP`
  - `Local Address`: Local IP:Port (e.g., `0.0.0.0:8080`)
  - `Foreign Address`: Remote destination IP:Port (e.g., `0.0.0.0:0` or `142.250.190.46:443`)
  - `State`: `LISTENING`, `ESTABLISHED`, or `TIME_WAIT`
  - `PID`: Process ID number (e.g., `4212` — use with `taskkill` to kill it!)
- **Example:** `netstat -ano | findstr :8080`

---

### `Test-NetConnection` (PowerShell Super Ping / Port Probe)
- **Definition:** Modern PowerShell command that tests both ICMP ping AND checks if a specific TCP port is open.
- **Advantage:** The best Windows tool to test if a database port (like 3306 or 5432) is open without installing Telnet.
- **What details you get when you type it:**
  - `ComputerName`: Target server
  - `RemoteAddress`: Resolved IP
  - `RemotePort`: Tested port number
  - `TcpTestSucceeded`: `True` (Port is OPEN!) or `False` (Port is BLOCKED / Closed!)
- **Example:** `Test-NetConnection -ComputerName db.example.com -Port 3306`

---

## 📁 2. File & Directory Navigation Commands

| Action | Windows CMD | Windows PowerShell | Linux Equivalent | What details you get |
| :--- | :--- | :--- | :--- | :--- |
| **List Files** | `dir` | `Get-ChildItem` (or `ls`) | `ls` | File names, sizes, and date modified |
| **List Hidden Files** | `dir /a` | `Get-ChildItem -Force` | `ls -la` | Displays hidden `.git`, system files, and dotfiles |
| **Search Subfolders**| `dir /s *.txt` | `Get-ChildItem -Recurse -Filter *.txt`| `find . -name "*.txt"` | All matching files across all folders |
| **Current Folder** | `cd` | `Get-Location` (or `pwd`)| `pwd` | Full folder path (e.g., `C:\Users\Admin`) |
| **Change Folder** | `cd <FOLDER>`| `Set-Location <FOLDER>`| `cd <FOLDER>` | Terminal moves to the new folder |
| **Go Up 1 Folder** | `cd ..` | `cd ..` | `cd ..` | Moves to parent folder |
| **Go to Drive** | `D:` | `Set-Location D:` | `cd /mnt/d` | Switches active drive letter |
| **Create Folder** | `mkdir <NAME>`| `New-Item -ItemType Directory <NAME>`| `mkdir <NAME>` | Creates the new directory |
| **Copy File** | `copy file.txt D:\`| `Copy-Item file.txt D:\` | `cp file.txt /dest/` | Duplicates file to destination |
| **Move / Rename** | `move old.txt new.txt`| `Move-Item old.txt new.txt`| `mv old.txt new.txt` | Renames or relocates file |
| **Delete File** | `del file.txt` | `Remove-Item file.txt` | `rm file.txt` | Permanently deletes file |
| **Delete Folder** | `rmdir /s /q <DIR>`| `Remove-Item -Recurse -Force <DIR>`| `rm -rf <DIR>` | Forcibly deletes folder and contents |
| **View File Content**| `type config.json`| `Get-Content config.json` (or `cat`)| `cat config.json` | Prints entire text file on screen |
| **Clear Screen** | `cls` | `Clear-Host` (or `cls`) | `clear` | Wipes the terminal screen clean |

---

## ⚙️ 3. Process, Task & System Control Commands

### `tasklist` (View All Running Programs)
- **Definition:** Lists every program, background service, and process running on Windows.
- **Advantage:** Discover which application is frozen or consuming memory.
- **What details you get when you type it:**
  - `Image Name`: Program name (e.g., `chrome.exe`, `node.exe`, `mysqld.exe`)
  - `PID`: Process ID number (e.g., `8540`)
  - `Session Name`: `Console` or `Services`
  - `Mem Usage`: RAM consumption (e.g., `450,120 K`)
- **Example:** `tasklist | findstr node`

---

### `taskkill /F /PID <PID>` (Force Kill Process)
- **Definition:** Forcibly terminates a process immediately using its Process ID number.
- **Advantage:** Unfreezes your computer by killing stuck servers, Node.js scripts, or background tasks.
- **Flags Breakdown:**
  - `/F`: Forcefully terminate (do not wait for app confirmation)
  - `/PID`: Specify the target Process ID number
- **What details you get when you type it:** `SUCCESS: The process with PID 8540 has been terminated.`
- **Example:** `taskkill /F /PID 8540`
- **Example by Name:** `taskkill /F /IM node.exe` (Kills all running Node processes)

---

### `systeminfo` (Full Computer Specifications)
- **Definition:** Generates a comprehensive summary of computer hardware, Windows OS build, BIOS, and RAM.
- **Advantage:** Check exact OS version, uptime, total RAM, and installed patches in one command.
- **What details you get when you type it:**
  - OS Name & Version (e.g., `Microsoft Windows 11 Pro 10.0.22631`)
  - System Manufacturer & Model
  - Processor / CPU specs
  - Total Physical Memory & Available Memory (RAM)
  - Network Card names and MAC addresses
- **Example:** `systeminfo`

---

### `shutdown /r /t 0` (Instant Restart)
- **Definition:** Reboots the computer immediately with zero second delay.
- **Flags Breakdown:**
  - `/r`: Reboot
  - `/t 0`: Time delay of 0 seconds
  - `/s`: Shutdown (power off)
- **Example:** `shutdown /r /t 0`

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Linux Commands Easy Guide](./01-Linux-Commands-Easy-Guide.md) | [Index](../README.md) | [Networking Commands & Concepts Easy Guide →](./03-Networking-Commands-and-Concepts-Easy-Guide.md) |
