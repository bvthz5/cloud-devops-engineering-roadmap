# 06 — CORS and Web Security Headers

Browsers enforce the **Same-Origin Policy (SOP)**: a script running on `https://frontend.com` cannot read data from `https://api.backend.com` unless the backend explicitly permits it via CORS.

---

## 1. The CORS Preflight Flow (`OPTIONS`)

When a browser makes a cross-origin request with custom headers or methods (PUT, DELETE):
```text
Browser                                                     API Server
   │                                                             │
   │ ─── 1. OPTIONS /api/data (Preflight) ─────────────────────> │
   │        Origin: https://frontend.com                         │
   │        Access-Control-Request-Method: DELETE                │
   │                                                             │
   │ <── 2. 204 No Content (Approval Headers) ────────────────── │
   │        Access-Control-Allow-Origin: https://frontend.com    │
   │        Access-Control-Allow-Methods: GET, POST, DELETE      │
   │                                                             │
   │ ─── 3. DELETE /api/data (Actual Request) ─────────────────> │
   │ <── 4. 200 OK ───────────────────────────────────────────── │
```

---

## 2. Production Security Headers

Add these headers at your reverse proxy (NGINX/Cloudflare):
- `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload` (Forces HTTPS).
- `X-Content-Type-Options: nosniff` (Prevents MIME-sniffing exploits).
- `X-Frame-Options: DENY` (Prevents clickjacking inside iframes).
- `Content-Security-Policy: default-src 'self'` (Prevents Cross-Site Scripting XSS).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - WebSockets & gRPC](./05-WebSockets-SSE-and-gRPC-Protocols.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
