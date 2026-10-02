# 01 - Nginx Architecture and Configuration

Nginx (pronounced "Engine-X") is an open-source, high-performance HTTP web server, reverse proxy, and Layer 4/7 load balancer. Engineered specifically to solve the C10K problem (handling 10,000 concurrent connections on commodity hardware), Nginx uses an asynchronous, non-blocking, event-driven architecture rather than allocating a dedicated operating system thread or process per connection.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Event-Driven Architecture & Worker Model](./01-Event-Driven-Architecture-and-Worker-Model.md) | Master-worker processes, `epoll`/`kqueue`, event loops, non-blocking I/O vs thread-per-connection. |
| 02 | [Configuration File Hierarchy & Contexts](./02-Configuration-File-Hierarchy-and-Contexts.md) | `main`, `http`, `server`, `location`, `upstream` contexts, inheritance rules, and modular `conf.d`. |
| 03 | [Location Block Matching Priority & Directives](./03-Location-Block-Matching-Priority-and-Directives.md) | Matching precedence (`=`, `^~`, `~`, `~*`, prefix), modifier regexes, and Pitfalls. |
| 04 | [URL Rewriting, Redirection & Returns](./04-URL-Rewriting-Redirection-and-Returns.md) | `rewrite` vs `return 301/302`, regex capture groups, `last`, `break`, `redirect`, `permanent` flags. |
| 05 | [Virtual Hosting & Server Blocks](./05-Virtual-Hosting-and-Server-Blocks.md) | Name-based vs IP-based virtual hosts, `server_name` wildcard & regex matching, default server catch-all. |
| 06 | [Nginx Performance Tuning & Kernel Directives](./06-Nginx-Performance-Tuning-and-Kernel-Directives.md) | `worker_processes auto`, `worker_connections`, `sendfile`, `tcp_nopush`, `tcp_nodelay`, open file limits. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Location regex precedence bypass, worker starvation, reload vs restart outage. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | `nginx -t`, error log levels, debug binaries, `strace` on worker processes, connection leaks. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on event-driven internals, caching, and scalability. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Multi-domain virtual host setup; Lab 2: Zero-downtime hot config reload; Lab 3: Performance tuning. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for syntax, location matching table, core flags, and CLI commands. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Master Index](../../00-Master-Index.md) | [README](./README.md) | [01 - Event-Driven Architecture](./01-Event-Driven-Architecture-and-Worker-Model.md) |
