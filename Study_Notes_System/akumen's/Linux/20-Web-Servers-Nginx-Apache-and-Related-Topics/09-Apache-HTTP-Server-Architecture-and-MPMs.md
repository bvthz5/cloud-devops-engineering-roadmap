# 9. Apache HTTP Server Architecture & Multi-Processing Modules (MPMs)

## Apache Process Architecture

Unlike Nginx's asynchronous event loop, Apache HTTP Server (`httpd` or `apache2`) relies on dynamic **Multi-Processing Modules (MPMs)** to control how incoming requests are bound to OS processes and threads.

```text
                     [ Apache Parent Process (root) ]
                                    │
         ┌──────────────────────────┼──────────────────────────┐
         ▼                          ▼                          ▼
  [ Prefork MPM ]            [ Worker MPM ]             [ Event MPM ]
(Process per request;     (Hybrid multi-process      (Asynchronous listener;
 isolated, high RAM)       + multi-threaded)         non-blocking Keep-Alive)
```

## Comparison of Apache MPMs

| Feature | Prefork MPM | Worker MPM | Event MPM (Modern Default) |
| --- | --- | --- | --- |
| **Model** | Multi-Process (No Threads) | Multi-Process Multi-Threaded | Hybrid Async Event-Driven |
| **Thread Safety** | Safe for non-thread-safe code (mod_php) | Requires thread-safe modules | Requires thread-safe modules |
| **Concurrency** | Low (High memory per connection) | Medium | High (Handles Keep-Alive async) |
| **Memory Footprint** | Very High | Medium | Low |

## `.htaccess` Files & Performance Impact

- **Function:** `.htaccess` allows directory-level configuration overrides without modifying root httpd configuration files.
- **Performance Trade-off:** When `AllowOverride All` is enabled, Apache is forced to search and parse `.htaccess` files in *every directory along the requested file path* for every single request.
- **Production Recommendation:** Disable `.htaccess` (`AllowOverride None`) in production and place all rules in main `<VirtualHost>` config blocks.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - WebSockets HTTP2 HTTP3 and gRPC Proxying](./08-WebSockets-HTTP2-HTTP3-and-gRPC-Proxying.md) | [README](./README.md) | [10 - Apache Virtual Hosts and Reverse Proxy Mod_Proxy](./10-Apache-Virtual-Hosts-and-Reverse-Proxy-Mod_Proxy.md) |
