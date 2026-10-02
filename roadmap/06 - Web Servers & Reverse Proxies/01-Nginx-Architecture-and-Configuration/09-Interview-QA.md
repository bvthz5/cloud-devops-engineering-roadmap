# 09 - Interview Questions & Architectural Scenarios

### Q1: How does Nginx achieve high concurrency compared to traditional process/thread-based web servers?
**Answer**: Nginx uses an asynchronous, non-blocking, event-driven architecture. A small number of single-threaded worker processes multiplex tens of thousands of connections using operating system event notification primitives (`epoll` on Linux, `kqueue` on BSD). A worker only spends CPU cycles when network data is ready to be processed, eliminating thread stack memory consumption and CPU context-switching overhead.

### Q2: What is the exact difference between `root` and `alias` directives?
**Answer**: With `root`, the full matched URI is appended to the root path (`root /var/www;` + `location /static/` ➔ `/var/www/static/file.css`). With `alias`, the matched location prefix is replaced by the alias path (`alias /var/www/;` + `location /static/` ➔ `/var/www/file.css`).

### Q3: Explain the significance of the `^~` location modifier.
**Answer**: `^~` designates a preferential prefix match. If Nginx determines that a `^~` prefix is the longest matching prefix for a request URI, it terminates the search immediately and **skips all regular expression matching**, even if matching regex blocks appear earlier in the configuration.

### Q4: What does the HTTP 444 status code represent in Nginx?
**Answer**: HTTP 444 is an Nginx-specific non-standard status code. When returned (e.g., `return 444;`), Nginx immediately closes the TCP connection without sending any HTTP headers or response payload back to the client. It is widely used in default catch-all blocks to block malicious bots and unmapped host headers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
