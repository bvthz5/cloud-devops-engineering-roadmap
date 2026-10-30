# 04 - URL Rewriting, Redirection, and Returns

## 1. `return` vs `rewrite`

For redirection and response generation, Nginx provides two directives: `return` and `rewrite`.

```text
Directive: return (Fast, Lightweight, High Performance)
- Directly halts request processing and returns an HTTP status code (301, 302, 403, 200).
- Recommended for all domain canonicalization (HTTP -> HTTPS, non-www -> www).

Directive: rewrite (Regex Parsing Engine)
- Rewrites internal URI paths dynamically using PCRE regular expressions.
- Executes within the Nginx rewrite phase. Higher CPU overhead.
```

---

## 2. Production Canonicalization: HTTP to HTTPS & Domain Enforcing

```nginx
# 1. Enforce HTTPS across all domains
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;

    # Return 301 Permanent Redirect instantly without regex
    return 301 https://$host$request_uri;
}

# 2. Enforce apex domain (strip www)
server {
    listen 443 ssl http2;
    server_name www.example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    return 301 https://example.com$request_uri;
}
```

---

## 3. The `rewrite` Directive and Flag Modifiers

Syntax: `rewrite regex replacement [flag];`

| Flag | Meaning | Use Case |
|---|---|---|
| `last` | Stops processing current rewrite set; **restarts location search** with new URI. | Rewriting paths that should be evaluated by other `location` blocks. |
| `break`| Stops processing current rewrite set; **does NOT restart location search**. | Serving files directly within current location block without re-routing. |
| `redirect` | Returns HTTP 302 Temporary Redirect. | Temporary link migration. |
| `permanent`| Returns HTTP 301 Permanent Redirect. | Permanent canonical path migration. |

```nginx
location /api/v1/ {
    # Rewrite /api/v1/users/42 -> /api/v2/users/42 and re-match locations
    rewrite ^/api/v1/(.*)$ /api/v2/$1 last;
}

location /download/ {
    # Rewrite path internally and serve file from disk immediately
    rewrite ^/download/(.*)$ /media/storage/$1 break;
    root /var/www;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Location Block Matching Priority and Directives](./03-Location-Block-Matching-Priority-and-Directives.md) | [Index](../../../README.md) | [05 - Virtual Hosting and Server Blocks →](./05-Virtual-Hosting-and-Server-Blocks.md) |
