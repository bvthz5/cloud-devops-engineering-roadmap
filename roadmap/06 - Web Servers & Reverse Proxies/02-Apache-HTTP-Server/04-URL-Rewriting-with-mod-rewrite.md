# 04 - URL Rewriting with mod_rewrite

## 1. `mod_rewrite` Engine and Syntax

`mod_rewrite` is Apache's URL manipulation engine. It executes rule-based rewrites using PCRE regular expressions.

```apache
<IfModule mod_rewrite.c>
    RewriteEngine On

    # 1. Enforce HTTPS Redirection
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

    # 2. Strip 'www' Prefix
    RewriteCond %{HTTP_HOST} ^www\.(.*)$ [NC]
    RewriteRule ^(.*)$ https://%1/$1 [L,R=301]

    # 3. Front Controller Pattern (SPA / Modern Frameworks like Laravel, Django)
    # If the requested file or directory does NOT exist on disk, route to index.php
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^ index.php [L,QSA]
</IfModule>
```

---

## 2. Common Rewrite Flags Reference

| Flag | Name | Function |
|---|---|---|
| `[L]` | **Last** | Stop processing further rules if this rule matched. |
| `[R=301]` | **Redirect** | Return HTTP 301 Permanent Redirect to the client browser. |
| `[NC]` | **No Case** | Case-insensitive regex matching. |
| `[QSA]` | **Query String Append**| Preserves existing query string parameters (`?user=123`) in the rewritten URL. |
| `[F]` | **Forbidden** | Return HTTP 403 Forbidden immediately. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Virtual Hosting and Name Based Routing](./03-Virtual-Hosting-and-Name-Based-Routing.md) | [Index](../../../README.md) | [05 - Reverse Proxying with mod proxy →](./05-Reverse-Proxying-with-mod-proxy.md) |
