# 16 - Quick Revision Cheat Sheet

High-density 5-minute summary for fast review before interviews, exams, or script writing.

---

## 🆚 The Core Difference

*   **`wget`**: A dedicated, robust file downloader. Handles recursive crawls and automatic retries. Defaults to saving files.
*   **`curl`**: A universal protocol communication tool. The industry standard for interacting with REST APIs. Defaults to outputting to the screen (`stdout`).

---

## 📥 File Downloading

| Task | `wget` Command | `curl` Command |
| :--- | :--- | :--- |
| **Save with original name** | `wget <URL>` | `curl -O <URL>` |
| **Save with custom name** | `wget -O name.zip <URL>` | `curl -o name.zip <URL>` |
| **Follow Redirects (301)**| *(Default)* | `curl -L <URL>` |
| **Resume Interrupted DL** | `wget -c <URL>` | `curl -C - -O <URL>` |
| **Silent Mode** | `wget -q <URL>` | `curl -sS <URL>` |
| **Ignore SSL Errors** | `wget --no-check-certificate` | `curl -k <URL>` |

> **Trap Warning:** `wget` uses `-O` (Capital) for custom names. `curl` uses `-o` (lowercase) for custom names, and `-O` (Capital) for original names.

---

## 🔄 `curl`: The REST API Standard

A typical API call combining multiple flags:

```bash
curl -sSL -X POST \
  -H "Authorization: Bearer my_token" \
  -H "Content-Type: application/json" \
  -d '{"key": "value"}' \
  https://api.example.com/v1/data | jq
```

*   **`-X <METHOD>`**: e.g., POST, PUT, DELETE.
*   **`-H "<HEADER>"`**: Inject custom HTTP headers.
*   **`-d '<DATA>'`**: Send payload (JSON, form data).
*   **`| jq`**: Pipe the output to the JSON processor for readability.

---

## 🪞 `wget`: The Mirroring Standard

Creating a local, offline copy of a static website:

```bash
wget -m -k -p https://example.com
```

*   **`-m`**: Mirror (Recursive, Infinite depth, Timestamp checking).
*   **`-k`**: Convert links to point to local files instead of the live internet.
*   **`-p`**: Download page requisites (images, CSS scripts) needed to display the HTML.

---

## 🐛 Troubleshooting

*   **`curl -v` (Verbose):** The ultimate debugging flag. Shows DNS resolution, SSL handshakes, and exactly what headers are sent (`>`) and received (`<`).
*   **`curl -I` (Headers Only):** Send an HTTP `HEAD` request to inspect server software, content length, or cookies without downloading the file body.
*   **"Works in Postman but not curl":** You probably forgot to add the `-H "Content-Type: application/json"` header, or your bash quotes around the JSON `-d` payload are messed up.

---

## 🎯 30-Second Interview Answer

> *"While both transfer data over networks, they serve different purposes. I use `wget` primarily for autonomous file downloading, as it handles retries automatically and can recursively mirror directories using flags like `-m`. Conversely, `curl` is my go-to for API interaction and network debugging. Because `curl` outputs to `stdout` and supports extensive manipulation of HTTP verbs (`-X`), headers (`-H`), and payloads (`-d`), it is the industry standard for writing REST API calls in bash scripts, usually piped into `jq` for parsing."*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [15 - MCQs](./15-MCQs.md) | [README](./README.md) | [17 - Related Topics](./17-Related-Topics.md) |
