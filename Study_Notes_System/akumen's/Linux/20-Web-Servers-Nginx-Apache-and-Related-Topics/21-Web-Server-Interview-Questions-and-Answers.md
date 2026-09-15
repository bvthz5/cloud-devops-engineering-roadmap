# 21. Web Server Interview Questions & Answers

1. **How does Nginx handle 10,000 concurrent requests with low memory compared to Apache Prefork?**
   - *Answer:* Nginx uses an asynchronous, non-blocking event-driven loop (`epoll`/`kqueue`) where a single worker process handles thousands of connections concurrently on one thread. Apache Prefork creates a dedicated OS process for every request, consuming megabytes of RAM per connection, leading to process thrashing and memory exhaustion.

2. **What is the difference between `proxy_pass http://127.0.0.1:8080` and `proxy_pass http://127.0.0.1:8080/` (trailing slash)?**
   - *Answer:* Without a trailing slash, Nginx passes the full request URI as received from the client. With a trailing slash, Nginx replaces the URI path matching the `location` directive with `/` when forwarding to upstream.

3. **What causes an HTTP 502 Bad Gateway error in Nginx?**
   - *Answer:* An HTTP 502 error occurs when Nginx acts as a proxy but receives an invalid response or connection failure from the upstream backend application (e.g., Node.js app is crashed or not listening on specified port).

4. **Why should `.htaccess` files be avoided in high-performance production Apache deployments?**
   - *Answer:* When `AllowOverride All` is enabled, Apache performs file system checks for `.htaccess` in every directory along the request path for *every request*, resulting in significant disk I/O penalties.
