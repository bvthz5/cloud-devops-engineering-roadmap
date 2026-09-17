# 13 - Interview Questions and Answers

Command-line networking tools are guaranteed to appear in technical assessments. 

---

## 🟢 Fundamentals

**Q1. What is the primary difference between `wget` and `curl`?**
> `wget` is primarily a robust downloader designed to fetch files, support resumes, and recursively mirror directories/websites. `curl` is a versatile communication tool designed to interact with APIs, support complex headers, HTTP methods (PUT/POST/DELETE), and output directly to the terminal (stdout).

---

**Q2. How do you save a file with a custom name using both tools?**
> In `wget`, use a capital `-O` (e.g., `wget -O custom.zip url`). 
> In `curl`, use a lowercase `-o` (e.g., `curl -o custom.zip url`).

---

**Q3. You are writing a bash script and `curl` is printing a progress bar that ruins your log output. How do you disable it?**
> Use the `-s` (silent) flag. It is often combined with `-S` so that errors are still printed if the request fails (`curl -sS`).

---

## 🔵 Intermediate Networking

**Q4. A URL redirects to a new location (HTTP 301). How do `wget` and `curl` handle this?**
> `wget` handles redirects automatically. `curl` does not follow redirects by default; you must use the `-L` (Location) flag to force it to follow the redirect to the final destination.

---

**Q5. You are testing an API. How do you send a JSON payload via a POST request in `curl`?**
> You must specify the POST method (`-X POST`), specify the JSON content type (`-H "Content-Type: application/json"`), and pass the payload (`-d '{"key":"value"}'`).

---

**Q6. You get an "SSL certificate problem" error when using `curl` against an internal company server. How do you bypass it temporarily for testing?**
> Use the `-k` or `--insecure` flag to ignore SSL certificate validation warnings.

---

## 🟠 Advanced & Scenario-Based

**Q7. How do you view the exact HTTP headers being sent and received in a `curl` request?**
> Add the `-v` (verbose) flag. It will display the DNS resolution, TLS handshake, request headers (prepended with `>`), and response headers (prepended with `<`).

---

**Q8. A massive 50GB database backup download via `wget` was interrupted at 40GB when the VPN dropped. How do you restart it without downloading the first 40GB again?**
> Rerun the exact same `wget` command but add the `-c` (continue) flag. `wget` will inspect the local file and send a Range request to the server to resume from that byte.

---

**Q9. You want to extract only the HTTP Status Code (e.g., 200 or 404) from a `curl` request inside a bash script, ignoring the response body. How?**
> You send the output body to `/dev/null` using `-o /dev/null`, run silently with `-s`, and use the write-out flag to extract the code: `-w "%{http_code}"`.

---

**Q10. How do you mirror a static website for offline viewing using `wget`?**
> Use `wget -m -k -p <URL>`. The `-m` mirrors the site recursively, `-p` downloads page requisites (CSS/images), and `-k` converts all the links to point to the local files so they work offline.

---

## 💡 30-Second Interview Answer

> *"While both `curl` and `wget` transfer data via the command line, their use cases differ. I use `wget` for autonomous, robust file downloads, especially when I need recursive website mirroring or reliable resume capabilities (`-c`). I use `curl` as my primary tool for interacting with REST APIs, testing webhooks, and troubleshooting network headers, utilizing its versatile flags for HTTP methods (`-X`), custom headers (`-H`), JSON payloads (`-d`), and verbose debugging (`-v`). In CI/CD pipelines, `curl` combined with `jq` is my standard for querying and parsing endpoints."*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Troubleshooting](./12-Troubleshooting.md) | [README](./README.md) | [14 - Hands On Terminal Practice](./14-Hands-On-Terminal-Practice.md) |
