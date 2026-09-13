# 02 - When to Use Each

Choosing between `wget` and `curl` often comes down to the specific task, personal preference, and what is installed on the target system. 

Here is a practical decision matrix for DevOps engineers.

---

## 🟢 Choose `wget` When:

1.  **Downloading Large Files:** If you are downloading a 5GB Linux ISO or a massive database dump, `wget` is the better choice. Its automatic retry mechanism means you can start it and walk away.
2.  **Recursive Downloading:** If you need to download all `.pdf` files from an open Apache directory index, `wget` can do this easily with its recursive flags. `curl` cannot.
3.  **Mirroring Websites:** If you need to create a static offline copy of a website, `wget` is the tool for the job.
4.  **Simple File Retrieval:** If you just want to grab a file and save it to the current directory with its original name, `wget` requires fewer flags than `curl`.

**Typical `wget` Use Case:**
```bash
# Grabbing a tarball for installation
wget https://example.com/software-v1.2.tar.gz
```

---

## 🔵 Choose `curl` When:

1.  **Interacting with REST APIs:** If you need to send GET, POST, PUT, or DELETE requests with JSON payloads and custom headers, `curl` is the industry standard.
2.  **Piping Output:** If you want to fetch a script or JSON data and immediately pipe it into another program (like `bash` or `jq`) without saving it to a file first.
3.  **Debugging Connections:** `curl` offers excellent verbose output (`-v`) for viewing the exact HTTP request and response headers, SSL handshakes, and network timing.
4.  **Advanced Authentication:** If you need to negotiate complex authentication schemes (OAuth, Bearer tokens, Kerberos), `curl` handles them better.
5.  **Non-HTTP Protocols:** If you need to upload a file via SCP or SFTP from the command line, `curl` supports it.

**Typical `curl` Use Case:**
```bash
# Testing an API endpoint
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' https://api.example.com/v1/data
```

---

## ⚖️ Summary Comparison Table

| Feature | `wget` | `curl` |
| :--- | :--- | :--- |
| **Default Output** | Saves to a file | Prints to standard output (screen) |
| **Automatic Retries**| Yes (Built-in and robust) | Requires specific flags (`--retry`) |
| **Recursive Download**| Yes | No |
| **REST APIs** | Possible, but clunky | Excellent (The industry standard) |
| **Protocols** | HTTP, HTTPS, FTP | 20+ protocols (HTTP, FTP, SCP, LDAP, etc.) |
| **Piping to bash** | Clunky (`wget -qO-`) | Native (`curl -s`) |
