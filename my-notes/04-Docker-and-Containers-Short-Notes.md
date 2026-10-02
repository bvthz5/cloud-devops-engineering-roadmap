# 🐳 Docker & Containers — Complete Kids-Mind Short Notes

> **Format:** Simple 1-line definitions, advantages, real-life analogies, and command breakdown. Covers every Docker concept, command, flag, and Dockerfile directive.

---

## 🧠 1. Core Container Concepts (1-Line Definitions)

| Concept | Simple 1-Line Definition (Kids Mind) | Real-Life Analogy | Main Advantage |
| :--- | :--- | :--- | :--- |
| **Docker** | A software platform that lets you build, test, and ship apps inside standardized containers. | An automated shipping company. | Runs identically on a developer laptop, testing server, or cloud VM. |
| **Container** | A running, isolated process packaged with its own code, libraries, and settings. | An apartment in a building sharing the ground and pipes. | Starts in seconds, uses very little RAM, and never interferes with other apps. |
| **Docker Image** | A frozen, read-only template with code and instructions used to launch containers. | A frozen pizza box ready to be baked. | Immutable; anyone who downloads this image gets the exact same program. |
| **Dockerfile** | A plain text file containing step-by-step instructions on how to build an image. | The printed recipe card for baking the pizza. | Automated, repeatable image builds tracked in Git. |
| **Docker Daemon (`dockerd`)** | The background engine that listens to Docker commands and builds/runs containers. | The factory manager doing all the heavy lifting. | Manages container lifecycles, memory, CPU, and network interfaces. |
| **Docker Client (`docker`)** | The command-line tool you type in terminal to talk to the Docker daemon. | The remote control that sends instructions to the TV. | User-friendly interface to control containers. |
| **Docker Hub / Registry** | An online library where people store, share, and download pre-built container images. | An online app store for container images. | Download official images like `ubuntu`, `python`, `node`, `nginx` instantly. |
| **Container Layer** | A thin, readable and writable layer placed on top of an image when a container runs. | A transparent sheet of plastic you write on with a marker. | Changes made inside a container are isolated and don't alter the base image. |

---

## 💻 2. Docker CLI Commands & Flags Breakdown

### 📦 Container Lifecycle Commands

#### `docker run` (Create & Start a Container)
- **1-Line Definition:** Downloads the image (if not local), creates a new container layer, and boots it up.
- **Advantage:** Launches an entire application stack in a single command.
- **Common Flags:**
  - `-d` (Detached): Runs container in the background so you keep your terminal prompt free.
  - `-it` (Interactive + TTY): Attaches your terminal keyboard directly inside the container (e.g., opens a bash shell).
  - `--name <NAME>`: Gives the container a human-friendly name instead of a random generated string.
  - `-p <HOST_PORT>:<CONTAINER_PORT>`: **Port Mapping**; bridges traffic from your computer port into the container port.
  - `-v <HOST_DIR>:<CONTAINER_DIR>`: **Volume Mount**; shares a folder between your host computer and container for persistent data.
  - `-e <KEY=VALUE>`: Injects an environment variable (like `DB_PASSWORD=secret`) into the container.
  - `--restart=always`: Automatically restarts the container if it crashes or if the server reboots.
  - `--rm`: Automatically deletes the container the moment it stops running (great for temporary test scripts).
  - `-m 512m`: Limits the container's maximum RAM usage to 512 Megabytes.
  - `--cpus 1.5`: Limits the container to using maximum 1.5 CPU cores.
  - `--network <NAME>`: Connects the container to a custom Docker virtual network.
- **What details you get when you type it:** Prints the full 64-character unique Container ID string.
- **Example:** `docker run -d --name my-web -p 8080:80 --restart=always nginx:alpine`

---

#### 🔌 Port Mapping Deep Dive (`-p <HOST_PORT>:<CONTAINER_PORT>`)
- **1-Line Definition:** Forwards incoming network packets from your physical computer/host VM into the private container port.
- **Why it is needed:** Containers live in isolated virtual subnets (`172.17.0.x`); without port mapping, your web browser cannot reach the container!
- **How to read `-p 8080:80`:**
  - `8080` (Left side): The port on YOUR real computer/laptop (what you type in your browser: `http://localhost:8080`).
  - `80` (Right side): The internal port the application listens on INSIDE the container.
- **Common Port Mapping Patterns:**
  - `docker run -p 80:80 nginx` ── Maps host port 80 directly to container 80.
  - `docker run -p 3000:3000 node-app` ── Maps host 3000 to Node.js app 3000.
  - `docker run -p 127.0.0.1:3306:3306 mysql` ── Security trick: only localhost can connect to MySQL; public internet is blocked!
  - `docker run -P nginx` ── Capital `-P` maps all exposed ports to random free high ports (e.g., `49153`).
- **What details you get when you check ports (`docker port <NAME>`):**
  ```text
  80/tcp -> 0.0.0.0:8080
  ```

---

#### 🔗 Container Linking: `--link` (Legacy) vs User-Defined Networks (Modern)
- **What was `--link`? (1-Line Definition):** The legacy Docker flag (`--link container_name:alias`) that manually injected an entry into `/etc/hosts` so two containers could talk.
- **Why `--link` is deprecated:** It was static, fragile, did not support automatic DNS, and required restarting containers if an IP changed.
- **The Modern DevOps Standard:** Use **Docker User-Defined Bridge Networks**:
  ```bash
  # Step 1: Create network
  docker network create my-app-net

  # Step 2: Attach database
  docker run -d --name db --network my-app-net postgres:16-alpine

  # Step 3: Attach backend (automatically resolves 'db' via built-in Docker DNS!)
  docker run -d --name backend --network my-app-net -e DB_HOST=db my-backend:v1
  ```
- **Advantage:** Automatic service discovery by container name; containers dynamically discover each other with zero IP hardcoding!

---

#### `docker ps` (List Running Containers)
- **1-Line Definition:** Shows all containers currently running on your machine.
- **Advantage:** Check what web servers or databases are alive.
- **What details you get when you type it:**
  1. `CONTAINER ID`: Short 12-character ID (e.g., `4a7b3c8d1e2f`)
  2. `IMAGE`: Image name and tag (e.g., `nginx:alpine`)
  3. `COMMAND`: The initial process running inside (e.g., `"nginx -g 'daemon of…"`)
  4. `CREATED`: When the container was created (e.g., `2 hours ago`)
  5. `STATUS`: Health & uptime status (e.g., `Up 2 hours`)
  6. `PORTS`: Port mappings (e.g., `0.0.0.0:8080->80/tcp`)
  7. `NAMES`: The container name (e.g., `my-web`)

#### `docker ps -a` (List All Containers Including Stopped)
- **1-Line Definition:** Lists every container on the system, including ones that stopped, exited, or crashed.
- **Advantage:** Find crashed containers, inspect exit codes, or locate containers to delete.
- **What details you get:** Same columns as `docker ps`, but `STATUS` shows `Exited (0) 5 minutes ago` or `Exited (137) OOMKilled`.

---

#### `docker stop <NAME_OR_ID>` (Graceful Stop)
- **1-Line Definition:** Sends `SIGTERM` to the container, waiting 10 seconds for it to close database connections and save data before shutting down.
- **Advantage:** Clean shutdown with zero data corruption.
- **What details you get:** Prints the name or ID of the stopped container.
- **Example:** `docker stop my-web`

#### `docker kill <NAME_OR_ID>` (Instant Force Kill)
- **1-Line Definition:** Sends `SIGKILL` to instantly terminate the container process without waiting.
- **Advantage:** Stops frozen, runaway, or uncooperative containers instantly.
- **Example:** `docker kill my-web`

#### `docker start <NAME_OR_ID>` (Start Existing Container)
- **1-Line Definition:** Boots up a container that was previously stopped without creating a new one.
- **Example:** `docker start my-web`

#### `docker restart <NAME_OR_ID>` (Reboot Container)
- **1-Line Definition:** Stops and immediately restarts the container.
- **Example:** `docker restart my-web`

#### `docker rm <NAME_OR_ID>` (Delete Stopped Container)
- **1-Line Definition:** Permanently deletes a stopped container from disk.
- **Advantage:** Frees up disk space.
- **Example:** `docker rm my-web`
- **Example (Force remove running container):** `docker rm -f my-web`

---

### 🔍 Container Inspection & Debugging Commands

#### `docker logs <NAME_OR_ID>` (View Container Output)
- **1-Line Definition:** Prints all standard output (`stdout`) and error messages (`stderr`) produced by the container.
- **Advantage:** Diagnose crashes, bugs, and error traces inside the container.
- **Useful Flags:**
  - `docker logs -f <NAME>`: Follow live streaming logs in real time.
  - `docker logs --tail 100 <NAME>`: Shows only the last 100 lines.
  - `docker logs -t <NAME>`: Shows timestamps on each log line.
- **Example:** `docker logs -f --tail 50 my-web`

---

#### `docker exec -it <NAME_OR_ID> sh` (Jump Inside Container Terminal)
- **1-Line Definition:** Opens a live, interactive shell prompt inside a running container.
- **Advantage:** Troubleshoot files, test database connections, and run diagnostics directly inside the container environment.
- **Flags Breakdown:**
  - `-i`: Interactive (keeps standard input open)
  - `-t`: Allocates a pseudo-TTY terminal window
- **Example:** `docker exec -it my-web /bin/sh` (or `/bin/bash`)

---

#### `docker inspect <NAME_OR_ID>` (Full JSON Configuration Dump)
- **1-Line Definition:** Returns every single low-level configuration detail of a container or image in JSON format.
- **Advantage:** Find the container's private IP address, volume mount paths, environment variables, and health check status.
- **What details you get:** IPAddress (e.g., `172.17.0.2`), Gateway, Mounts, State, RestartPolicy, and Env vars.
- **Example:** `docker inspect my-web | grep "IPAddress"`

---

#### `docker stats` (Live Resource Monitor)
- **1-Line Definition:** A live terminal dashboard showing real-time CPU, RAM, Network I/O, and Disk I/O consumption for every running container.
- **Advantage:** Find out which container is hogging CPU or running out of memory.
- **What details you get:** `CONTAINER ID`, `NAME`, `CPU %`, `MEM USAGE / LIMIT`, `MEM %`, `NET I/O`, `BLOCK I/O`, `PIDS`.
- **Example:** `docker stats`

---

#### `docker cp` (Copy Files In/Out of Containers)
- **1-Line Definition:** Copies files or folders between your host filesystem and a running container.
- **Advantage:** Extract debug logs, dump crash files, or inject emergency configuration fixes without restarting.
- **Examples:**
  - Copy from host to container: `docker cp ./nginx.conf my-web:/etc/nginx/nginx.conf`
  - Copy from container to host: `docker cp my-web:/var/log/nginx/error.log ./error.log`

---

#### `docker top <NAME>` (Inspect Running Processes)
- **1-Line Definition:** Lists the processes actively running inside a specific container, including their Host PIDs.
- **Advantage:** Verify if child worker processes, databases, or daemons are running inside without entering the container.
- **What details you get:** `UID`, `PID`, `PPID`, `C`, `STIME`, `TTY`, `TIME`, `CMD`.
- **Example:** `docker top my-web`

---

#### `docker commit <CONTAINER> <NEW_IMAGE>` (Save Container State)
- **1-Line Definition:** Creates a new frozen Docker image from an existing container's current state and changes.
- **Advantage:** Quick snapshot of an experimentally modified container for debugging or forensics.
- **Example:** `docker commit my-web my-user/custom-nginx:v1`

---

#### `docker save` & `docker load` (Offline Image Backup & Transfer)
- **1-Line Definition:** `docker save` exports an image to a `.tar` archive file; `docker load` imports the `.tar` back into Docker on any offline machine.
- **Advantage:** Transfer container images to air-gapped secure servers without needing internet or Docker Hub!
- **Examples:**
  - Save: `docker save -o my-app.tar my-user/app:v1.0`
  - Load: `docker load -i my-app.tar`

---

### 🖼️ Docker Image Management Commands

| Command | Simple 1-Line Definition | Common Flags & Usage | What Details Appear on Screen |
| :--- | :--- | :--- | :--- |
| **`docker images`** | Lists all container images stored locally on your machine. | `docker images` | `REPOSITORY`, `TAG`, `IMAGE ID`, `CREATED`, `SIZE` |
| **`docker pull <IMAGE>`** | Downloads an image from Docker Hub to your local computer. | `docker pull node:20-alpine` | Progress bars downloading each image layer |
| **`docker push <IMAGE>`** | Uploads your custom image to Docker Hub or private registry. | `docker push myuser/app:v1.0` | Upload progress per layer + final SHA digest |
| **`docker tag <SRC> <DEST>`** | Creates a new alias name or version tag pointing to an image. | `docker tag app:latest app:v2.0` | Silent; creates pointer in local image index |
| **`docker rmi <IMAGE>`** | Deletes a local image from disk. | `docker rmi node:20-alpine` | Untags and deletes image layers |
| **`docker build -t <NAME> .`**| Compiles a Dockerfile in the current directory into a new image.| `-t app:v1` (tag name), `-f Dockerfile.prod` | Step-by-step build logs for each directive |

---

### 🧹 Docker Prune & Disk Cleanup Commands

| Command | Simple 1-Line Definition | Advantage |
| :--- | :--- | :--- |
| **`docker system df`** | Shows how much disk space is consumed by images, containers, local volumes, and build cache. | See exactly what is wasting gigabytes. |
| **`docker container prune`** | Deletes all stopped containers at once. | Cleans up zombie container layers. |
| **`docker image prune`** | Deletes all dangling images (untagged `<none>:<none>` layers). | Reclaims build cache space safely. |
| **`docker volume prune`** | Deletes all unused volumes not attached to any container. | Deletes orphaned database storage. |
| **`docker network prune`** | Deletes all custom networks not used by at least one container. | Cleans up stale virtual bridges. |
| **`docker system prune -af --volumes`** | **The Ultimate Nuke Command**: Deletes ALL stopped containers, all unused networks, all dangling and unused images, and all volumes. | Reclaims 50GB+ of disk space in one shot! |

---

## 📜 3. Dockerfile Directives Breakdown

Every directive in a `Dockerfile` creates a cached layer. Here is every directive explained simply:

```dockerfile
# 1. FROM: The starting base image (Operating System or Runtime)
FROM node:20-alpine

# 2. WORKDIR: Sets the working directory inside the container (like cd)
WORKDIR /app

# 3. COPY: Copies files from your host computer into the container filesystem
COPY package*.json ./

# 4. RUN: Executes commands during image compilation (installs packages, compiles code)
RUN npm ci --only=production

# 5. COPY application code
COPY . .

# 6. ENV: Sets persistent environment variables inside the container
ENV NODE_ENV=production
ENV PORT=3000

# 7. EXPOSE: Documentation hint indicating which port the app listens on
EXPOSE 3000

# 8. USER: Security best practice - runs the app as non-root user
USER node

# 9. CMD: The default command executed when the container starts running
CMD ["node", "server.js"]
```

### Directives Comparison Cheat Sheet:

| Directive | What It Does (1 Line) | When It Runs | Example |
| :--- | :--- | :--- | :--- |
| **`FROM`** | Defines the parent base image to build upon. | Build Time | `FROM python:3.12-slim` |
| **`WORKDIR`** | Creates and switches into an internal folder for all subsequent commands. | Build & Run Time | `WORKDIR /usr/src/app` |
| **`COPY`** | Copies local files from your project folder into the image. | Build Time | `COPY . /app` |
| **`ADD`** | Like `COPY`, but can also unpack `.tar.gz` archives and fetch remote URLs. | Build Time | `ADD app.tar.gz /app` |
| **`RUN`** | Executes terminal commands inside the image and commits the resulting layer. | Build Time | `RUN apt-get update && apt-get install -y curl` |
| **`ENV`** | Sets environment variables accessible during build AND runtime. | Build & Run Time | `ENV DB_HOST=db.internal` |
| **`ARG`** | Sets temporary build-time variables that disappear in the final image. | Build Time Only | `ARG VERSION=1.0` |
| **`EXPOSE`** | Documents which network port the container plans to listen on. | Metadata Only | `EXPOSE 8080` |
| **`VOLUME`** | Declares a mount point for external persistent data storage. | Run Time | `VOLUME ["/var/lib/mysql"]` |
| **`USER`** | Switches user away from root for security compliance. | Build & Run Time | `USER 10001` |
| **`ENTRYPOINT`** | The fixed executable binary that runs when container boots. | Run Time | `ENTRYPOINT ["nginx"]` |
| **`CMD`** | Default arguments passed to `ENTRYPOINT` (can be easily overridden in CLI). | Run Time | `CMD ["-g", "daemon off;"]` |
| **`HEALTHCHECK`**| Tests if the containerized app is truly working, not just process running. | Run Time | `HEALTHCHECK CMD curl -f http://localhost/ || exit 1` |

---

### ❓ What is the Difference between `CMD` and `ENTRYPOINT`?
- **`ENTRYPOINT` is the fixed program** (e.g., `git`). It cannot be overridden without `--entrypoint`.
- **`CMD` is the default argument** (e.g., `status`). If the user types `docker run my-image pull`, `pull` overrides `status`!
- **Best Practice:** Use `ENTRYPOINT` for the program and `CMD` for default flags!

---

## 🚫 4. `.dockerignore` File

### What is `.dockerignore`?
- **1-Line Definition:** A text file listing files and folders that Docker must NEVER copy into the container image during `docker build`.
- **Advantage:** Prevents uploading huge folders like `node_modules`, keeps image sizes small, and prevents leaking `.env` passwords into public images!

### Standard `.dockerignore` Template:
```text
# Ignore local dependencies (let container install them clean)
node_modules
__pycache__
*.pyc

# Ignore secret files
.env
.env.local
id_rsa
*.pem

# Ignore Git history & build outputs
.git
.gitignore
dist/
build/
.DS_Store
*.log
```

---

## 🗄️ 5. Docker Volumes & Persistent Storage

- **The Problem:** By default, containers are **ephemeral**; when a container is deleted, all files created inside it are destroyed forever!
- **The Solution:** Use **Volumes** to store data on the host machine outside the container.

| Storage Type | Simple 1-Line Definition | Typical Use Case | Command Syntax |
| :--- | :--- | :--- | :--- |
| **Named Volume** | Managed by Docker inside `/var/lib/docker/volumes/`; safe from accidental deletion. | Production databases (MySQL, Postgres, Redis). | `docker run -v db_data:/var/lib/mysql mysql` |
| **Bind Mount** | Directly maps an exact folder from your host computer into the container. | Live local coding (code edits on host reflect instantly inside container). | `docker run -v $(pwd)/src:/app/src my-app` |
| **tmpfs Mount** | Stores data strictly in host RAM memory; never touches the hard drive. | Sensitive temporary secrets or high-speed cache. | `docker run --tmpfs /tmp my-app` |

---

## 🌐 6. Docker Networking

Docker provides virtual software switches (bridges) to connect containers:

| Network Driver | Simple 1-Line Definition | Default / Usage |
| :--- | :--- | :--- |
| **`bridge`** | Default virtual network switch inside a single Docker host. Containers get private `172.17.x.x` IPs. | Used by 95% of standard multi-container web apps. |
| **`host`** | Removes network isolation; container shares host computer's exact network and IP directly. | High-performance apps where port forwarding overhead is unacceptable. |
| **`none`** | Completely disables networking inside the container (no network card, loopback only). | Air-gapped batch calculation jobs or cryptographic key generation. |
| **`overlay`** | Multi-host network connecting containers across different physical machines. | Powers Docker Swarm and Kubernetes pod communication. |
| **`macvlan`** | Assigns a physical MAC address to container, making it look like a physical machine on your router. | Legacy enterprise applications requiring direct LAN presence. |

---

## 🐙 7. Docker Compose

### What is Docker Compose?
- **1-Line Definition:** A tool for defining and running multi-container Docker applications using a single YAML file (`docker-compose.yml`).
- **Advantage:** Launch a complete application (Frontend + Backend + PostgreSQL + Redis) with one command (`docker compose up -d`).

### Sample `docker-compose.yml`:
```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    depends_on:
      - api
    restart: always

  api:
    build: ./api
    environment:
      - DB_HOST=db
      - DB_PORT=5432
    depends_on:
      - db

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: appdb
      POSTGRES_PASSWORD: secretpassword
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

### Essential Docker Compose Commands:
- `docker compose up -d`: Builds, creates, and starts all services in the background.
- `docker compose down`: Stops and deletes all containers and virtual networks created by the file.
- `docker compose down -v`: Deletes containers, networks, AND named storage volumes.
- `docker compose ps`: Shows the status and ports of all services in this compose project.
- `docker compose logs -f`: Streams live unified logs from all services simultaneously.
- `docker compose restart`: Restarts all containers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Networking Easy Guide](./03-Networking-Commands-and-Concepts-Easy-Guide.md) | [Index](../README.md) | [Kubernetes (K8s) Short Notes →](./05-Kubernetes-K8s-Short-Notes.md) |
