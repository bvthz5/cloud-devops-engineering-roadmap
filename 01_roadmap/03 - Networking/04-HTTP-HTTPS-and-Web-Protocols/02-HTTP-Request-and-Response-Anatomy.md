# 02 — HTTP Request and Response Anatomy

HTTP follows a client-server request-response architecture.

---

## 1. HTTP Methods & Idempotency

- **Idempotent:** Making the same request multiple times produces the exact same server state as making it once.
- **Safe:** Does not modify any resource on the server.

| Method | Purpose | Safe? | Idempotent? | Typical DevOps Use |
| :--- | :--- | :---: | :---: | :--- |
| **`GET`** | Retrieve resource representation | Yes | Yes | Fetching API data, health checks |
| **`POST`** | Create new resource / trigger action | No | No | Submitting forms, triggering builds |
| **`PUT`** | Completely replace an existing resource | No | Yes | Overwriting full configuration |
| **`PATCH`**| Partially update an existing resource | No | No | Updating specific JSON field |
| **`DELETE`**| Remove resource | No | Yes | Destroying VM or container |
| **`HEAD`** | Same as GET, but returns headers only | Yes | Yes | Fast health check probes |
| **`OPTIONS`**| Query supported methods (CORS preflight) | Yes | Yes | Browser cross-origin validation |

---

## 2. HTTP Status Code Families

- **`1xx` (Informational):** `101 Switching Protocols` (Upgrading to WebSockets).
- **`2xx` (Success):** `200 OK`, `201 Created`, `204 No Content`.
- **`3xx` (Redirection):** `301 Moved Permanently` (Cached), `302 Found` (Temporary), `304 Not Modified` (Cache hit).
- **`4xx` (Client Error):**
  - `400 Bad Request`: Malformed JSON or invalid syntax.
  - `401 Unauthorized`: Missing or invalid authentication token.
  - `403 Forbidden`: Authenticated, but lacks required permissions.
  - `404 Not Found`: Resource does not exist.
  - `429 Too Many Requests`: Rate limit exceeded.
- **`5xx` (Server Error):**
  - `500 Internal Server Error`: Application unhandled exception.
  - `502 Bad Gateway`: Reverse proxy received an invalid response or connection refused from backend.
  - `503 Service Unavailable`: Server overloaded or undergoing maintenance.
  - `504 Gateway Timeout`: Reverse proxy timed out waiting for backend application.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - HTTP Protocol Evolution 1.1 2.0 3.0](./01-HTTP-Protocol-Evolution-1.1-2.0-3.0.md) | [Index](../../../README.md) | [03 - HTTPS TLS Termination and Certificates →](./03-HTTPS-TLS-Termination-and-Certificates.md) |
