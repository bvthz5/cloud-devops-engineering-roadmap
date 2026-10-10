# 03 - Handlers & Event-Driven Notification Architecture

Handlers in Ansible provide an event-driven mechanism to trigger reactive operations—such as service reloads, daemon restarts, cache flushes, and firewall updates—only when an actual configuration change occurs on the target system. Rather than restarting critical production daemons on every run, handlers guarantee **de-duplication** and execute only when notified.

---

## 1. Handlers Architecture: The Notification & De-Duplication Engine

In traditional imperative shell scripts, modifying three separate Nginx configuration files causes three separate service restarts, causing unnecessary connection drops for users:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    HANDLER DE-DUPLICATION VS. PROCEDURAL SCRIPTS                │
│                                                                                 │
│   ❌ PROCEDURAL SCRIPT (3 Restarts)             ✅ ANSIBLE HANDLER (1 Restart)   │
│                                                                                 │
│   Task 1: Update nginx.conf                     Task 1: Update nginx.conf       │
│   ==> service nginx restart (Drop 1)            ==> notify: Restart Nginx       │
│                                                                                 │
│   Task 2: Update proxy.conf                     Task 2: Update proxy.conf       │
│   ==> service nginx restart (Drop 2)            ==> notify: Restart Nginx       │
│                                                                                 │
│   Task 3: Update certs                          Task 3: Update certs            │
│   ==> service nginx restart (Drop 3)            ==> notify: Restart Nginx       │
│                                                                                 │
│   RESULT: 3 Downtimes!                          END OF PLAY:                    │
│                                                 Handler runs ONCE at the end!   │
│                                                 RESULT: Zero wasted restarts!   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The 3 Core Rules of Handlers:
1. **Triggered Only on `changed`:** If a task returns `ok` (e.g., config file already matched desired state), the notification is ignored.
2. **De-Duplicated Execution:** No matter how many tasks notify the same handler during a play, the handler executes **exactly once**.
3. **End-of-Play Execution:** By default, all notified handlers run sequentially at the very end of the play, after all standard tasks have completed.

---

## 2. Production Playbook Implementation with Handlers

```yaml
---
- name: Deploy Secure Reverse Proxy Fleet
  hosts: webservers
  become: yes
  force_handlers: yes # SRE Best Practice: Run handlers even if later tasks fail!

  tasks:
    - name: Deploy main Nginx configuration
      ansible.builtin.template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        owner: root
        group: root
        mode: '0644'
      notify: Restart Nginx

    - name: Deploy virtual host configuration
      ansible.builtin.template:
        src: templates/app_vhost.conf.j2
        dest: /etc/nginx/sites-available/app.conf
        mode: '0644'
      notify: Reload Nginx # Reload is faster and preserves active TCP connections!

    - name: Deploy custom TLS certificate
      ansible.builtin.copy:
        src: files/wildcard.crt
        dest: /etc/ssl/certs/app.crt
        mode: '0640'
      notify:
        - Reload Nginx
        - Audit Certificate Update

  handlers:
    - name: Restart Nginx
      ansible.builtin.systemd:
        name: nginx
        state: restarted

    - name: Reload Nginx
      ansible.builtin.systemd:
        name: nginx
        state: reloaded

    - name: Audit Certificate Update
      ansible.builtin.command: logger "TLS certificate updated by Ansible deployment"
      changed_when: false
```

---

## 3. Decoupling Handlers with `listen` Topics

In large enterprise playbooks and reusable roles, tasks should not be tightly coupled to a specific handler's string name. The **`listen`** directive allows multiple handlers to subscribe to an abstract pub/sub topic:

```yaml
tasks:
  - name: Update database connection string
    ansible.builtin.lineinfile:
      path: /etc/app/env
      line: "DB_HOST=10.0.2.10"
    notify: "restart web services" # Notifies topic

handlers:
  # Both handlers listen on the same topic and execute in order!
  - name: Gracefully reload PHP-FPM worker pool
    ansible.builtin.systemd:
      name: php8.2-fpm
      state: reloaded
    listen: "restart web services"

  - name: Reload Nginx HTTP reverse proxy
    ansible.builtin.systemd:
      name: nginx
      state: reloaded
    listen: "restart web services"
```

---

## 4. Mid-Play Handler Execution: `meta: flush_handlers`

Because handlers normally wait until the end of the play, problems arise when a subsequent task depends on the newly restarted service:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       THE MID-PLAY DEPENDENCY PROBLEM                           │
│                                                                                 │
│   Task 1: Update PostgreSQL listen address from 127.0.0.1 to 0.0.0.0            │
│           (Notifies: Restart PostgreSQL)                                        │
│   Task 2: Run application database schema migration                             │
│           (Attempts to connect to PostgreSQL over TCP port 5432)                │
│                                                                                 │
│   💥 FAILURE: PostgreSQL has NOT restarted yet! It is still listening only     │
│      on localhost! Task 2 throws Connection Refused!                            │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### The SRE Solution: Flushing Handlers Immediately
Use the built-in **`meta: flush_handlers`** task to force all pending notified handlers to run immediately before continuing:

```yaml
tasks:
  - name: Update PostgreSQL configuration
    ansible.builtin.template:
      src: postgresql.conf.j2
      dest: /etc/postgresql/16/main/postgresql.conf
    notify: Restart PostgreSQL

  # Flush handlers right now!
  - name: Force immediate execution of pending handlers
    ansible.builtin.meta: flush_handlers

  # Now this task succeeds because PostgreSQL is already listening on 0.0.0.0!
  - name: Verify PostgreSQL TCP port is open and accepting traffic
    ansible.builtin.wait_for:
      port: 5432
      delay: 2
      timeout: 30

handlers:
  - name: Restart PostgreSQL
    ansible.builtin.systemd:
      name: postgresql
      state: restarted
```

---

## 5. The Critical SRE Failure Mode: `force_handlers`

By default, if any task in a play fails, **Ansible aborts execution immediately, and pending handlers NEVER run**. 

### The Disaster Scenario:
1. Task 1 updates `/etc/nginx/nginx.conf` with new SSL paths (Notifies: `Restart Nginx`).
2. Task 2 fails with a syntax error or network timeout.
3. Ansible halts the play. The handler is **never executed**.
4. The server is left in an inconsistent state: new files on disk, old process in memory. When the server reboots days later, Nginx fails to start, causing an unexpected outage!

### The SRE Hardening Checklist:
1. **Always Set `force_handlers = True` in `ansible.cfg`:**
   ```ini
   [defaults]
   force_handlers = True
   ```
2. **Or Add `force_handlers: yes` at the Play Level:** Guarantees that if Task 1 modified files, the notified handler runs even if Task 2 fails.
3. **Use `ansible-playbook --force-handlers`:** Enforce from the CLI during manual operational runs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Idempotency Principles and Changed Status](./02-Idempotency-Principles-and-Changed-Status.md) | [Index](../../../README.md) | [04 - Conditionals when failed_when changed_when →](./04-Conditionals-when-failed_when-changed_when.md) |

