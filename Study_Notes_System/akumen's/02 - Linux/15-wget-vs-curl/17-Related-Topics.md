# 17 - Related Topics

Once you are comfortable with `wget` and `curl`, you will encounter situations where specialized network tools are more efficient.

---

## 🦎 `jq` (Command-line JSON processor)

**What it is:** A lightweight and flexible command-line JSON processor. It is the companion tool to `curl`.
**Why it matters:** APIs return dense, unformatted JSON strings. `jq` parses them, colorizes them, and allows you to extract specific keys.
*   **Format output:** `curl -s api.com/data | jq`
*   **Extract specific value:** `curl -s api.com/data | jq '.user.id'`

---

## 🥧 `HTTPie` (`http`)

**What it is:** A modern, user-friendly command-line HTTP client written in Python. It is designed to be a simpler alternative to `curl`.
**Why it matters:** `curl` syntax can be clunky. `HTTPie` features built-in JSON support, colorized formatting, and intuitive syntax without needing `jq` or complex `-H` flags.
*   *curl:* `curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' api.com`
*   *HTTPie:* `http POST api.com key=value`

---

## 🗡️ `netcat` (`nc`)

**What it is:** The "Swiss Army knife" of networking. It reads and writes data across network connections using TCP or UDP.
**Why it matters:** While `curl` speaks HTTP, `netcat` speaks raw TCP. It is used for port scanning, testing firewall rules, creating simple chat servers, or testing if a service is reachable at a lower level than HTTP.
*   **Test port connectivity:** `nc -zv example.com 22` (Checks if SSH port is open).

---

## 📡 `ping`, `traceroute`, and `mtr`

**What it is:** ICMP-based network diagnostic tools.
**Why it matters:** If `curl` times out, you need to know *where* the connection is failing. 
*   **`ping`** checks if the host is reachable and measures latency.
*   **`traceroute`** shows every router hop between you and the server. 
*   **`mtr`** combines both into a real-time, continually updating network diagnostic screen. (Crucial for proving to your ISP that packet loss is occurring).

---

## 🌐 `nmap`

**What it is:** A network exploration tool and security / port scanner.
**Why it matters:** If `curl example.com:8080` returns "Connection Refused", you might use `nmap` to scan the entire server to see exactly which ports *are* actually open and what services are running on them.
*   **Scan standard ports:** `nmap example.com`

---

## 📦 `rsync` and `scp`

**What it is:** Tools specifically designed for transferring files securely over SSH.
**Why it matters:** While `curl` can upload via SFTP/SCP, `rsync` and `scp` are the industry standards for moving files between Linux servers. `rsync` is particularly powerful because it only transfers the *differences* between files, drastically reducing bandwidth usage for backups.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [16 - Quick Revision](./16-Quick-Revision.md) | [README](./README.md) | [README (Index)](./README.md) |
