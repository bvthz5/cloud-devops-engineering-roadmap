# 05 - curl: REST API Requests

While `wget` can technically interact with APIs, `curl` is the undisputed king of REST. It is the universal language used in API documentation worldwide.

By default, `curl` sends an HTTP `GET` request. To interact with REST APIs, you often need to change the HTTP method (Verb) and send data payloads (usually JSON).

---

## 🔄 Changing the HTTP Method (`-X`)

Use the `-X` flag to specify the HTTP request method.

```bash
# Explicit GET (default, not strictly necessary)
curl -X GET https://api.example.com/users

# DELETE request
curl -X DELETE https://api.example.com/users/123
```

---

## 📦 Sending Data Payloads (`-d`)

Use the `-d` (or `--data`) flag to send data in the body of the request. This is primarily used with `POST` and `PUT` methods.

### 1. Sending URL-encoded Form Data
If you don't specify a content type, `curl` assumes you are sending standard web form data (`application/x-www-form-urlencoded`).

```bash
curl -X POST -d "username=admin&password=123" https://api.example.com/login
```

### 2. Sending JSON Data
Modern APIs require JSON. You must pass the JSON string using `-d`, AND you must tell the server you are sending JSON by setting the `Content-Type` header using `-H`.

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "role": "DevOps"}' \
  https://api.example.com/users
```
*(Notice the single quotes around the JSON payload to protect the double quotes inside from being interpreted by the bash shell).*

### 3. Sending Data from a File
If your JSON payload is large, don't type it in the terminal. Put it in a file (e.g., `payload.json`) and tell `curl` to read the file using the `@` symbol.

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d @payload.json \
  https://api.example.com/users
```

---

## 📬 Receiving JSON Responses

API responses are often unformatted, dense JSON strings.

### The DevOps Standard: Piping to `jq`
Because `curl` outputs directly to `stdout`, the standard practice is to pipe the output into `jq` (a command-line JSON processor) for pretty-printing and colorization.

```bash
curl -s https://api.example.com/users | jq
```
*(Remember the `-s` flag for silent mode, so `curl`'s progress meter doesn't break the JSON formatting before it hits `jq`).*

---

## 📝 Summary: The Ultimate REST API Command

A standard API call usually combines Silent mode, Follow Redirects, a custom Method, Headers, and Data, piped to `jq`.

```bash
curl -sSL -X PUT \
  -H "Authorization: Bearer my_token" \
  -H "Content-Type: application/json" \
  -d '{"status": "active"}' \
  https://api.example.com/users/123 | jq
```
