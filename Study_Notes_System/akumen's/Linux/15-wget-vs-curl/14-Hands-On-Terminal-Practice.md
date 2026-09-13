# 14 - Hands-On Terminal Practice

This lab provides safe exercises to practice using both tools against public endpoints.

> **Prerequisites:** A Linux/macOS terminal with `wget` and `curl` installed.

---

## 🛠️ Level 1: Basic Downloads and Redirects

**Tasks:**
1. Download the `robots.txt` file from Google using `wget`.
2. Download the same file using `curl`, but save it as `google_robots.txt`.
3. Try to download a file from `http://google.com` (which redirects to HTTPS). Observe how both tools handle it.

**Commands:**
```bash
# 1. Basic wget
wget https://www.google.com/robots.txt

# 2. Basic curl with custom output
curl -o google_robots.txt https://www.google.com/robots.txt

# 3. Redirects
wget http://google.com  # Succeeds (automatically follows to https)
curl http://google.com  # Fails (returns a 301 HTML body)
curl -L http://google.com # Succeeds (Follows redirect)
```

---

## ⬛ Level 2: Interacting with REST APIs

We will use `https://httpbin.org`, a free service designed for testing HTTP requests.

**Tasks:**
1. Send a basic GET request to `https://httpbin.org/get`.
2. Send a POST request to `https://httpbin.org/post` with a JSON payload: `{"role": "devops"}`. Ensure you set the correct `Content-Type` header.

**Commands:**
```bash
# 1. Simple GET (Output goes to terminal)
curl https://httpbin.org/get

# 2. JSON POST Request
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"role": "devops"}' \
  https://httpbin.org/post
```
*(Look closely at the JSON response from httpbin; it will echo back the "data" you sent, proving it worked).*

---

## 🟫 Level 3: Debugging and Headers

**Tasks:**
1. View only the HTTP response headers from `github.com`.
2. Run a highly verbose request to `https://httpbin.org/headers` and find the TLS version used in the handshake.
3. Send a request to `https://httpbin.org/user-agent` spoofing your browser as "Windows 95".

**Commands:**
```bash
# 1. Fetch only headers
curl -I https://github.com

# 2. Verbose debugging (look for lines starting with *)
curl -v https://httpbin.org/headers

# 3. Spoofing User-Agent
curl -A "Windows 95" https://httpbin.org/user-agent
# Or the long way: curl -H "User-Agent: Windows 95" ...
```

---

## 🟦 Level 4: The CI/CD Readiness Probe

**Task:**
Write a one-liner `curl` command that fetches `https://httpbin.org/status/200`. It must be completely silent, discard the response body, and ONLY print the HTTP status code.

**Command:**
```bash
curl -s -o /dev/null -w "%{http_code}\n" https://httpbin.org/status/200
```
*(Try changing the URL to `/status/404` or `/status/500` to see the output change).*

---

## 🟥 Level 5: Mirroring with `wget`

**Task:**
Create a local, offline backup of `http://example.com`.

**Command:**
```bash
mkdir example_mirror
cd example_mirror

# Mirror, page requisites, convert links
wget -m -p -k http://example.com

# Explore the resulting folder
ls -la example.com/
```
