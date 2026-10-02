# 10 — Hands-On Practice Labs: Linux Package Management

Complete these four practical, production-grade labs to master package management across Debian, RHEL, and Alpine environments.

---

## Lab 1: Securely Adding a Third-Party GPG Signed Repository (Modern Debian/Ubuntu)

### Objective
Install the latest official upstream Redis server on Ubuntu 22.04/24.04 using secure isolated keyrings without using deprecated `apt-key`.

### Steps:
1. Install prerequisites:
   ```bash
   sudo apt-get update
   sudo apt-get install -y ca-certificates curl gnupg lsb-release
   ```
2. Create dedicated keyrings directory:
   ```bash
   sudo install -m 0755 -d /etc/apt/keyrings
   ```
3. Fetch the public GPG key, de-armor it, and store it with read-only permissions:
   ```bash
   curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/redis-archive-keyring.gpg
   sudo chmod a+r /etc/apt/keyrings/redis-archive-keyring.gpg
   ```
4. Add the repository to `/etc/apt/sources.list.d/redis.list` with explicit `signed-by`:
   ```bash
   echo "deb [signed-by=/etc/apt/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
   ```
5. Update and verify the package source:
   ```bash
   sudo apt-get update
   apt-cache policy redis-server
   ```
6. Install and verify:
   ```bash
   sudo apt-get install -y redis-server
   redis-server --version
   ```

---

## Lab 2: DNF History Rollback & Version Locking (RHEL/Rocky/Fedora)

### Objective
Install an application, inspect the DNF transaction history database, rollback the transaction, and lock an essential database package to prevent accidental upgrades.

### Steps:
1. Install an example package:
   ```bash
   sudo dnf install -y htop
   ```
2. Check the transaction log:
   ```bash
   dnf history
   ```
3. Inspect details of the latest transaction:
   ```bash
   dnf history info $(dnf history | head -n 4 | tail -n 1 | awk '{print $1}')
   ```
4. Rollback the installation:
   ```bash
   sudo dnf history undo -y last
   which htop || echo "htop cleanly uninstalled via transaction rollback!"
   ```
5. Version-lock NGINX to protect production stability:
   ```bash
   sudo dnf install -y python3-dnf-plugin-versionlock nginx
   sudo dnf versionlock add nginx
   sudo dnf versionlock list
   ```
6. Test upgrading:
   ```bash
   sudo dnf upgrade -y nginx
   # Notice DNF respects versionlock and skips upgrade!
   ```

---

## Lab 3: Alpine Linux Ephemeral Build Dependencies in Docker

### Objective
Write an optimized container build stage that compiles a C program on Alpine Linux without retaining the heavy compiler toolchain in the final image layer.

### Steps:
Create a file named `Dockerfile.alpine-lab`:
```dockerfile
FROM alpine:3.19

WORKDIR /app

RUN echo -e '#include <stdio.h>\nint main(){ printf("Production Hello from Compiled Alpine Binary!\\n"); return 0; }' > app.c

# Install build dependencies inside an isolated virtual group
RUN apk add --no-cache --virtual .build-deps gcc musl-dev \
    && gcc -O2 -o /usr/local/bin/myapp app.c \
    && rm -f app.c \
    && apk del .build-deps

CMD ["/usr/local/bin/myapp"]
```

Build and test:
```bash
docker build -t alpine-build-lab -f Dockerfile.alpine-lab .
docker run --rm alpine-build-lab
docker images alpine-build-lab
# Notice the total image size is under 15MB!
```

---

## Lab 4: Compiling HAProxy from Source with Custom Flags

### Objective
Compile HAProxy 3.0 from source code, install into `/usr/local`, and verify linked shared libraries using `ldd`.

### Steps:
1. Install build dependencies on Ubuntu:
   ```bash
   sudo apt-get update
   sudo apt-get install -y build-essential libpcre3-dev zlib1g-dev libssl-dev
   ```
2. Download and unpack source tarball:
   ```bash
   cd /tmp
   curl -fsSL https://www.haproxy.org/download/3.0/src/haproxy-3.0.0.tar.gz -o haproxy.tar.gz
   tar -xzf haproxy.tar.gz
   cd haproxy-3.0.0
   ```
3. Compile with optimized target flags:
   ```bash
   make TARGET=linux-glibc USE_OPENSSL=1 USE_PCRE=1 USE_ZLIB=1 -j$(nproc)
   ```
4. Install binary and documentation:
   ```bash
   sudo make install PREFIX=/usr/local
   ```
5. Verify installation and dynamically linked libraries:
   ```bash
   /usr/local/sbin/haproxy -v
   ldd /usr/local/sbin/haproxy
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
