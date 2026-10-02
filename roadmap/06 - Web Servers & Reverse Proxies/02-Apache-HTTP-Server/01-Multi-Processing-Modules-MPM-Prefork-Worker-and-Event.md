# 01 - Multi-Processing Modules (MPM): Prefork, Worker, and Event

## 1. Apache MPM Architecture

Apache delegates network connection handling and process scheduling to pluggable **Multi-Processing Modules (MPMs)**. Choosing the correct MPM determines Apache's scalability, memory footprint, and thread safety.

```text
+-----------------------------------------------------------------------------------+
|                        Apache MPM Comparison Matrix                               |
+-------------------+-------------------------------+-------------------------------+
| MPM               | Architecture                  | Best Used For                 |
+-------------------+-------------------------------+-------------------------------+
| mpm_prefork       | Multi-process, single thread  | Non-thread-safe modules       |
|                   | per process. Zero threading.  | (Legacy mod_php). Heavy RAM!  |
+-------------------+-------------------------------+-------------------------------+
| mpm_worker        | Multi-process, multi-threaded | High-traffic servers with     |
|                   | per process.                  | thread-safe modules.          |
+-------------------+-------------------------------+-------------------------------+
| mpm_event         | Multi-process, multi-threaded | Modern standard. Solves slow  |
|                   | + dedicated listener thread   | keepalive blocking with async |
|                   | for idle keepalive sockets.   | event loops (epoll).          |
+-------------------+-------------------------------+-------------------------------+
```

---

## 2. Deep Dive: `mpm_prefork` vs `mpm_event`

### 2.1 The `mpm_prefork` Flaw
In `prefork`, each incoming connection is handled by an isolated OS process. If a client maintains an idle HTTP keepalive connection for 60 seconds, that entire OS process (~20-50MB RAM) sits completely idle, unable to serve any other client! Under 2,000 concurrent keepalive connections, the server requires 80GB of RAM or crashes into OOM swap.

### 2.2 The `mpm_event` Solution
The `event` MPM assigns a dedicated **listener thread** per child process. When a request finishes and enters the keepalive state, the worker thread is returned to the pool, and the socket is monitored by the listener thread via `epoll`. Only when new data arrives is a worker thread re-assigned!

```text
mpm_event Architecture:
[ Incoming Requests ] ──► [ Listener Thread (epoll) ]
                               │
            ┌──────────────────┼──────────────────┐
            ▼                  ▼                  ▼
     [ Worker Thread 1 ] [ Worker Thread 2 ] [ Worker Thread N ]
     (Serves Request)    (Serves Request)    (Returns to Pool on Keepalive)
```

---

## 3. Configuring `mpm_event` for Production

```apache
# /etc/apache2/mods-available/mpm_event.conf
<IfModule mpm_event_module>
    StartServers             3
    MinSpareThreads          75
    MaxSpareThreads          250
    ThreadsPerChild          25
    MaxRequestWorkers        400
    MaxConnectionsPerChild   0
</IfModule>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Configuration Hierarchy](./02-Configuration-Hierarchy-and-Directive-Scopes.md) |
