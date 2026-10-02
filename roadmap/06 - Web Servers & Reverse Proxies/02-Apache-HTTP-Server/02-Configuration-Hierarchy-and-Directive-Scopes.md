# 02 - Configuration Hierarchy and Directive Scopes

## 1. Directory and Location Scopes

Apache configuration files (`httpd.conf` / `apache2.conf`) utilize XML-like container tags to scope directives to filesystem paths or request URLs.

| Container Tag | Target Evaluated | Performance Impact |
|---|---|---|
| `<Directory "/path">` | **Filesystem Directory** | Evaluated on startup / cached. Fast. |
| `<Files "regex">` | **Specific Filenames** | Evaluated per file within directory. |
| `<Location "/url">` | **Request Web URL Path** | Evaluated per HTTP request after routing. |

```apache
# Recommended Security Baseline
<Directory />
    AllowOverride None
    Require all denied
</Directory>

<Directory "/var/www/html">
    Options -Indexes +FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

---

## 2. The High Performance Cost of `.htaccess`

The `.htaccess` file allows non-root developers to override web server rules at the directory level. However, **`.htaccess` incurs a severe performance degradation in production**:

```text
When AllowOverride is enabled:
Request: GET /var/www/html/app/images/logo.png

Apache must check for the existence of .htaccess in EVERY parent folder:
1. stat("/.htaccess")
2. stat("/var/.htaccess")
3. stat("/var/www/.htaccess")
4. stat("/var/www/html/.htaccess")
5. stat("/var/www/html/app/.htaccess")
6. stat("/var/www/html/app/images/.htaccess")

Result: 6 disk I/O stat() system calls for EVERY single asset request!
```

### Production Best Practice:
Always disable `.htaccess` globally in production by setting:
```apache
AllowOverride None
```
and define all rewrite rules and access controls directly inside `<Directory>` blocks in the main configuration.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Multi Processing Modules MPM Prefork Worker and Event](./01-Multi-Processing-Modules-MPM-Prefork-Worker-and-Event.md) | [Index](../../../README.md) | [03 - Virtual Hosting and Name Based Routing →](./03-Virtual-Hosting-and-Name-Based-Routing.md) |
