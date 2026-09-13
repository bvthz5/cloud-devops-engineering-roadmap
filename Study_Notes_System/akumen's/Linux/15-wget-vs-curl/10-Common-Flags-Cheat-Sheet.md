# 10 - Common Flags Cheat Sheet

A quick reference guide for the most frequently used flags in both tools. Remember that flags are case-sensitive.

---

## 🛠️ `curl` Cheat Sheet

| Flag | Purpose | Example |
| :--- | :--- | :--- |
| **`-O`** | Save file with its remote name. | `curl -O http://site.com/file.zip` |
| **`-o`** | Save file with a custom name. | `curl -o custom.zip http://site.com/file.zip` |
| **`-L`** | Follow HTTP 301/302 redirects. | `curl -L http://short.url/xyz` |
| **`-I`** | Fetch only the HTTP headers (HEAD request). | `curl -I http://example.com` |
| **`-v`** | Verbose mode (debug connection/headers). | `curl -v http://example.com` |
| **`-X`** | Specify HTTP method (GET, POST, PUT, DELETE). | `curl -X POST http://api.com/data` |
| **`-d`** | Send data payload (implies POST). | `curl -d '{"key":"val"}' http://api.com` |
| **`-H`** | Pass a custom header (Content-Type, Auth). | `curl -H "Auth: Bearer 123" http://api.com` |
| **`-F`** | Submit multipart form data (File upload). | `curl -F "file=@/path/to/pic.jpg" http://api.com` |
| **`-u`** | Basic Authentication (user:password). | `curl -u admin:1234 http://api.com` |
| **`-s`** | Silent mode (hides progress bar/errors). | `curl -s http://api.com` |
| **`-S`** | Show errors even if silent mode (`-s`) is on. | `curl -sS http://api.com` |
| **`-k`** | Insecure (ignore SSL certificate warnings). | `curl -k https://untrusted.com` |
| **`-C -`**| Resume an interrupted transfer. | `curl -C - -O http://site.com/file.zip` |

---

## 🧰 `wget` Cheat Sheet

| Flag | Purpose | Example |
| :--- | :--- | :--- |
| **`-O`** | Save file with a custom name. | `wget -O custom.zip http://site.com/file.zip` |
| **`-c`** | Continue/resume an interrupted download. | `wget -c http://site.com/file.zip` |
| **`-q`** | Quiet mode (hides progress bar). | `wget -q http://site.com/file.zip` |
| **`-r`** | Recursive download (crawling). | `wget -r http://site.com/docs/` |
| **`-m`** | Mirror entire website. | `wget -m http://example.com` |
| **`-k`** | Convert links for local offline viewing. | `wget -m -k http://example.com` |
| **`-p`** | Download page requisites (CSS/Images). | `wget -m -p http://example.com` |
| **`-A`** | Accept list (only download specific extensions). | `wget -r -A.pdf http://site.com` |
| **`--no-check-certificate`** | Insecure (ignore SSL warnings). | `wget --no-check-certificate https://untrusted.com` |
| **`--limit-rate`**| Limit bandwidth usage. | `wget --limit-rate=500k http://site.com/file.iso` |
