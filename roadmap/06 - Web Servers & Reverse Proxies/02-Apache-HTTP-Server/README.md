# 02 - Apache HTTP Server

The Apache HTTP Server (historically known as `httpd`) is one of the foundational software systems of the internet. While Nginx and Envoy dominate modern cloud-native edge routing, Apache remains widely deployed across enterprise legacy environments, cPanel/shared hosting architectures, and applications requiring dynamic directory-level configuration via `.htaccess` or rich module ecosystems like `mod_security`, `mod_auth_kerb`, and `mod_perl`.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Multi-Processing Modules (MPM): Prefork, Worker & Event](./01-Multi-Processing-Modules-MPM-Prefork-Worker-and-Event.md) | MPM architectures, process vs thread concurrency, memory models, event MPM async loop. |
| 02 | [Configuration Hierarchy & Directive Scopes](./02-Configuration-Hierarchy-and-Directive-Scopes.md) | `httpd.conf`/`apache2.conf`, `Directory`, `Files`, `Location`, `.htaccess` performance cost. |
| 03 | [Virtual Hosting & Name-Based Routing](./03-Virtual-Hosting-and-Name-Based-Routing.md) | `<VirtualHost *:80>`, ServerName, ServerAlias, DocumentRoot, and default vhost behavior. |
| 04 | [URL Rewriting with mod_rewrite](./04-URL-Rewriting-with-mod-rewrite.md) | `RewriteEngine`, `RewriteCond`, `RewriteRule` flags (`[L]`, `[R=301]`, `[NC]`, `[QSA]`). |
| 05 | [Reverse Proxying with mod_proxy](./05-Reverse-Proxying-with-mod-proxy.md) | `ProxyPass`, `ProxyPassReverse`, `mod_proxy_balancer`, `ProxyPreserveHost`, connection pooling. |
| 06 | [Security Hardening & Module Management](./06-Security-Hardening-and-Module-Management.md) | Disabling ServerTokens, `AllowOverride None`, `mod_security` WAF, TLS hardening via `mod_ssl`. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Prefork OOM collapse, runaway `.htaccess` stat() calls, ProxyPass leak. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | `apachectl configtest`, `apache2ctl -S`, log analysis, checking active MPM (`apache2ctl -V`). |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on Apache internals, MPM comparisons, and tuning. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Migrating from prefork to event MPM; Lab 2: Multi-vhost reverse proxy; Lab 3: Hardening. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for syntax, directives, rewrite flags, and CLI commands. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Nginx Architecture](../01-Nginx-Architecture-and-Configuration/README.md) | [README](./README.md) | [01 - Multi-Processing Modules](./01-Multi-Processing-Modules-MPM-Prefork-Worker-and-Event.md) |
