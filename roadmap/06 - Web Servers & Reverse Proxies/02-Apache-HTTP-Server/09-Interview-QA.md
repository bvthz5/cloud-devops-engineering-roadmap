# 09 - Interview Questions & Architectural Scenarios

### Q1: Compare Apache `mpm_prefork` and `mpm_event`. When is prefork still required?
**Answer**: `mpm_prefork` creates a separate non-threaded process for every connection, consuming high RAM per connection and suffering under keepalive idle locks. It is only required when using non-thread-safe legacy modules (like embedded `mod_php`). `mpm_event` is a modern multi-threaded hybrid model with dedicated async listener threads for keepalives, making it fast and memory-efficient.

### Q2: Why should `AllowOverride None` be set globally in production environments?
**Answer**: When `AllowOverride` is enabled, Apache checks for the existence of a `.htaccess` file in every parent directory up the filesystem tree for every single requested resource, generating multiple redundant `stat()` system calls that severely degrade disk I/O performance.

### Q3: What is the function of `ProxyPreserveHost On` in `mod_proxy`?
**Answer**: By default, `mod_proxy` replaces the incoming client `Host` header with the hostname of the backend server. Setting `ProxyPreserveHost On` passes the original client `Host` header to the backend application, which is mandatory for virtual hosting and SSL redirection on backend servers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
