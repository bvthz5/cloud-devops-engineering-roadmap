# 07 - Authentication

Interacting with secured APIs or downloading files from restricted directories requires authentication.

---

## 🔐 1. Basic Authentication

Basic Authentication sends the username and password encoded in Base64 within the HTTP headers. It is the oldest and simplest form of web authentication.

### Using `curl` (`-u`)
The `-u` flag takes the format `username:password`.

```bash
curl -u admin:supersecret123 https://api.example.com/protected
```
*(Security Note: Passing passwords on the command line is dangerous because it gets saved in your `.bash_history`. It is safer to omit the password; `curl -u admin` will prompt you to type the password securely).*

### Using `wget`
`wget` uses distinct flags for user and password.

```bash
wget --http-user=admin --http-password=supersecret123 https://example.com/protected.zip
```

---

## 🎟️ 2. Bearer Tokens (OAuth / JWT)

Modern APIs rarely use Basic Auth. Instead, you authenticate by passing a secret Token (often a JSON Web Token or JWT) in the HTTP Headers.

Because Bearer Tokens are just HTTP headers, you use the `-H` flag in `curl` (or `--header` in `wget`).

### Using `curl`
```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5..." https://api.example.com/v1/data
```

### Passing Tokens Securely
To avoid saving your token in shell history, you should export it as an environment variable and reference the variable in your `curl` command.

```bash
export API_TOKEN="eyJhbGciOiJIUzI1NiIsInR5..."
curl -H "Authorization: Bearer $API_TOKEN" https://api.example.com/v1/data
```

---

## 🍪 3. Cookies

Sometimes authentication relies on session cookies rather than tokens or Basic Auth.

### Sending a Cookie in `curl` (`-b`)
If you have the cookie string:
```bash
curl -b "session_id=8987654321; user=admin" https://example.com/dashboard
```

### Saving Cookies (`-c`) and Reusing Them (`-b`)
If you need to log in to a site and then download a file, you can tell `curl` to save the cookies from the login response to a file (cookie jar), and then use that file for the next request.

```bash
# 1. Log in and save the cookies to 'cookies.txt' (-c)
curl -c cookies.txt -d "user=admin&pass=123" https://example.com/login

# 2. Use those saved cookies to access the protected file (-b)
curl -b cookies.txt -O https://example.com/secure_report.pdf
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Headers and Debugging](./06-Headers-and-Debugging.md) | [README](./README.md) | [08 - File Uploads](./08-File-Uploads.md) |
