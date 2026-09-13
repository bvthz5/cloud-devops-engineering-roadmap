# 04 - Redirects and Resumes

Real-world downloads rarely go perfectly. Links move (HTTP 301/302 Redirects) and connections drop. Knowing how to handle these situations is critical.

---

## 🔀 Handling Redirects

Many URLs you download from (like GitHub release links or URL shorteners) don't point directly to the file. They return an HTTP 301 or 302 redirect to the actual file location.

### Using `wget` (Automatic)
`wget` is smart. It follows redirects automatically by default. You don't need to do anything.

```bash
wget http://github.com/user/repo/releases/latest
# wget automatically follows the redirect to the actual v1.0.tar.gz file.
```

### Using `curl` (Requires `-L`)
`curl` is literal. If you request a URL that issues a redirect, `curl` will simply download the HTML body of the redirect message (usually empty or a small "Document Moved" string) and stop.

To force `curl` to follow redirects (Location headers), you must use the **`-L`** flag.

```bash
curl -L -O http://github.com/user/repo/releases/latest
```
*(Without `-L`, you would download an empty file or a small HTML error page).*

---

## ⏯️ Resuming Interrupted Downloads

If you are downloading a 10GB file and your VPN drops at 9GB, you do not want to start over. Both tools support HTTP range requests to resume downloads.

### Using `wget` (`-c`)
Use the `-c` (continue) flag. If `wget` finds a partially downloaded file in the current directory with the same name, it will ask the server to resume from that exact byte.

```bash
wget -c https://example.com/huge_file.iso
```
*(Note: As mentioned in Chapter 1, `wget` will automatically retry on its own if the connection drops while it is actively running. The `-c` flag is used if `wget` was actually killed or stopped, and you are restarting the command manually).*

### Using `curl` (`-C -`)
Use the `-C -` flag. The `-` tells `curl` to automatically figure out where the local file left off and resume from there.

```bash
curl -C - -O https://example.com/huge_file.iso
```

> **Server Support Required:** Both of these resume features require the remote web server to support "Range Requests" (specifically the `Accept-Ranges: bytes` header). Most modern servers do, but if the server refuses, both tools will start the download from the beginning.
