# 08 - User Applications & Services

At the top of the User Space execution layer sit **User Applications and Background Services (Daemons)**. These software programs perform real-world business logic, process web requests, manage databases, or provide containerized microservices.

---

## 🏬 Types of User Applications

### 1. Interactive Applications
Programs launched directly by users via terminal or GUI:
- Text Editors (`vim`, `nano`, `code`).
- Browsers (`firefox`, `chrome`).
- Developer Toolchains (`python3`, `gcc`, `go`, `git`).

### 2. Background Service Daemons
Non-interactive processes running continuously in background user space, managed by systemd:
- Web Servers (`nginx`, `apache2`).
- Database Engines (`mysqld`, `postgres`).
- SSH Server (`sshd`).

### 3. Containerized Runtimes & Workloads
- Docker Engine (`dockerd`), `containerd`, and Kubernetes Kubelet (`kubelet`).

---

## ⚙️ Process Isolation & System Calls

Even complex user applications (like a high-performance Nginx web server or PostgreSQL database) run completely in unprivileged **Ring 3**. They cannot access hardware directly.

When Nginx receives an incoming HTTP web request:
1. Nginx calls `accept()` via `glibc` to receive the network socket connection from the kernel network stack.
2. Nginx reads static files from disk using `read()` system calls.
3. Nginx sends data back across the socket using `sendfile()` system calls.

---

## ⬅️ Navigation
- Previous: [07 - The Shell](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/07-The-Shell.md)
- Next: [09 - Command Traversal Flow: `ls`, `cat`, Networking](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/07-Core-Components-of-a-Linux-Machine/09-Command-Flow-ls-cat-networking.md)
