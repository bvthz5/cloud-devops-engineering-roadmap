# 01 - wget vs curl: Fundamentals

Both `wget` and `curl` are command-line utilities used to download and upload data via a network. They are non-interactive, meaning they can run in the background or be triggered by scripts without user input.

However, they were built for different primary purposes.

---

## 🕸️ `wget` (Web Get)

Created in 1996 as part of the GNU Project, `wget` is designed primarily for **downloading files**.

### Key Characteristics:
1.  **Autonomous:** `wget` is designed to be robust. If a download fails due to network instability, `wget` will automatically retry until the file is fully retrieved.
2.  **Recursive:** `wget` can spider through a webpage, follow links, and download entire directories or mirror entire websites for offline viewing.
3.  **Simplicity:** It does one thing (fetching files) and does it extremely well.
4.  **Protocols:** Supports HTTP, HTTPS, and FTP.
5.  **Output:** By default, it saves the output to a file in the current directory.

---

## 🥌 `curl` (Client URL)

Created in 1997 (originally named `urlget`), `curl` is powered by the `libcurl` library. It is designed as a universal tool for **transferring data** to or from a server.

### Key Characteristics:
1.  **Bi-directional:** While `wget` mostly downloads, `curl` is heavily used for uploading data, submitting forms, and making complex API requests.
2.  **Protocol Agnostic:** `curl` supports an insane number of protocols: DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET, and TFTP.
3.  **No Recursion:** `curl` does not follow links or mirror websites. It fetches exactly what you tell it to fetch, once.
4.  **Piping:** By default, `curl` outputs the received data to `stdout` (the terminal screen), making it ideal for piping into other tools like `grep` or `jq`.
5.  **Library Backing:** Because `curl` is built on `libcurl`, its underlying engine is embedded in countless other software applications and languages (like PHP's cURL extension).

---

## 🛠️ Installation

Both tools are nearly ubiquitous on Linux, but depending on your distribution (especially minimal Docker images), one might be missing.

```bash
# Ubuntu / Debian
sudo apt install wget curl

# CentOS / RHEL
sudo yum install wget curl

# Alpine (Common in Docker)
apk add wget curl
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - When to use each](./02-When-to-use-each.md) |
