# 06 - Headers and Debugging

When network requests fail, the error is rarely in the payload itself. It is usually in the HTTP headers, or a failure in the TCP/TLS handshake. `curl` provides exceptional tools for manipulating and viewing these layers.

---

## 🏷️ Setting Custom Headers (`-H`)

HTTP headers pass metadata between the client and server. Common headers include `Content-Type`, `Authorization`, and `User-Agent`.

Use the `-H` flag in `curl` to pass a header. You can use multiple `-H` flags in a single command.

```bash
curl -H "X-Custom-Header: DevOps-Test" \
     -H "Accept-Language: en-US" \
     https://api.example.com/data
```

### Spoofing the User-Agent
Some web servers block command-line tools like `curl` or `wget`. You can bypass this by pretending to be a normal web browser.

**In `curl`:**
```bash
curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64)" https://example.com
```

**In `wget`:**
```bash
wget -U "Mozilla/5.0 (Windows NT 10.0; Win64; x64)" https://example.com
```

---

## 🔬 Viewing Response Headers (`-I` or `-i`)

Sometimes you don't want the actual file or HTML body; you just want to see the HTTP response headers (e.g., to check the server type, content length, or cookies).

### Fetch Only the Headers (`-I`)
This sends an HTTP `HEAD` request. The server returns the headers, but not the body.
```bash
curl -I https://example.com
```
*Output snippet:*
```text
HTTP/2 200 
content-type: text/html; charset=UTF-8
server: nginx
date: Mon, 14 Sep 2026 10:00:00 GMT
```

### Fetch Body AND Headers (`-i`)
This sends a standard `GET`, but prints the headers before printing the body.
```bash
curl -i https://example.com
```

---

## 🐛 Verbose Debugging (`-v`)

If a connection is failing (e.g., hanging, TLS certificate error, or dropping instantly), add the **`-v`** (verbose) flag. It is the most useful troubleshooting flag in `curl`.

```bash
curl -v https://example.com
```

**Understanding Verbose Output:**
Lines starting with different characters indicate the direction of communication:
*   `*` : Informational (DNS resolution, IP connection, SSL/TLS handshake details).
*   `>` : Request headers sent BY `curl` TO the server.
*   `<` : Response headers sent BY the server TO `curl`.

If you need even more detail (including a hex dump of the raw data transfer), use `--trace-ascii -`.

---

## ⏱️ Analyzing Connection Time (Performance Testing)

DevOps engineers often use `curl` to test API latency. You can format `curl`'s output to show exactly where time is being spent using the `-w` (write-out) flag.

```bash
curl -o /dev/null -s -w "DNS: %{time_namelookup}s \nConnect: %{time_connect}s \nTTFB: %{time_starttransfer}s \nTotal: %{time_total}s \n" https://example.com
```
*   **TTFB (Time To First Byte):** The time it took the server to process the request and start responding. Extremely useful for performance tuning.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - curl REST API Requests](./05-curl-REST-API-Requests.md) | [README](./README.md) | [07 - Authentication](./07-Authentication.md) |
