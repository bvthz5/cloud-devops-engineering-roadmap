# 12 — Modern CLI Replacements & Productivity Power Tools

While classic POSIX tools (`grep`, `find`, `cat`, `ls`) are universal, modern systems engineering has produced a new generation of high-speed, Rust- and Go-based CLI utilities that drastically accelerate daily productivity.

---

## 1. Modern vs Classic CLI Tool Matrix

| Task | Classic POSIX Tool | Modern DevOps Replacement | Written In | Key Advantage |
| :--- | :--- | :--- | :--- | :--- |
| **Code Search** | `grep -r` | **`ripgrep` (`rg`)** | Rust | **10x faster**, multi-threaded, respects `.gitignore` by default |
| **File Search** | `find . -name` | **`fd`** | Rust | Simple syntax (`fd pattern`), colorized, ignores hidden/.git |
| **File Viewer** | `cat` | **`bat`** | Rust | Syntax highlighting, line numbers, git diff integration |
| **Directory List**| `ls -la` | **`eza`** (formerly `exa`) | Rust | Git status indicators, tree view, human-friendly formatting |
| **Fuzzy Finding** | `Ctrl+R` / `history` | **`fzf`** | Go | Interactive real-time fuzzy completion for files, git, and history |
| **JSON Querying** | `awk` / Python | **`jq`** | C | Universal JSON query and transformation standard |
| **YAML Querying** | Python / Ruby | **`yq`** | Go | Native YAML, JSON, XML, and TOML manipulation |
| **HTTP Client** | `curl -i` | **`httpie`** or **`curlie`** | Python/Go | Formatted JSON display, simple intuitive syntax |
| **Process Monitor**| `top` | **`htop`** / **`btop`** | C/C++ | Visual CPU cores, memory bars, mouse support, tree view |

---

## 2. Fast Text Search with `ripgrep` (`rg`)

`ripgrep` is universally recognized as the fastest search tool in computing:

```bash
# Search for string recursively in current project (skips .git and vendor dirs)
rg "DATABASE_PORT"

# Case-insensitive search across specific file types (.py and .yaml only)
rg -i "redis_url" -t py -t yaml

# Search for regex pattern and replace in terminal output
rg -e "v[0-9]+\.[0-9]+"

# Search hidden files and ignored files when needed
rg -u "secret_token"
```

---

## 3. Fast File Finding with `fd`

No more memorizing complex `find . -type f -name "*.go"` incantations:

```bash
# Find all files matching 'config'
fd config

# Find only directories matching 'logs'
fd -t d logs

# Find all YAML files modified in the last 2 hours
fd -e yaml --changed-within 2h

# Execute command on each found file (e.g., delete all temp files)
fd -e tmp -x rm -v {}
```

---

## 4. Enhanced File Viewing with `bat`

`bat` replaces `cat` with full syntax highlighting, automatic Git integration, and paging:

```bash
# View source code or configuration with syntax highlighting
bat /etc/nginx/nginx.conf

# View plain without headers or line numbers (drop-in cat replacement in scripts)
bat -p deploy.yaml
```

---

## 5. Interactive Fuzzy Finding with `fzf`

`fzf` turns any text stream into an interactive, live fuzzy-search menu:

```bash
# 1. Fuzzy search and open file in Vim
vim $(fzf)

# 2. Interactive Git Branch Checkout
git checkout $(git branch | fzf)

# 3. Interactive Kill Process Menu
kill -9 $(ps -ef | fzf | awk '{print $2}')

# 4. Interactive Kubernetes Pod Log Inspection
kubectl logs -f $(kubectl get pods -o name | fzf)
```

---

## 6. JSON Data Wrangling with `jq`

In cloud engineering, almost every API (AWS CLI, GitHub API, Docker daemon) speaks JSON. Mastering `jq` is mandatory:

```bash
# Sample JSON payload:
# curl -s https://api.github.com/repos/kubernetes/kubernetes

# 1. Pretty-print and colorize JSON
curl -s https://api.github.com/repos/kubernetes/kubernetes | jq .

# 2. Extract a specific top-level field
curl -s https://api.github.com/repos/kubernetes/kubernetes | jq '.stargazers_count'

# 3. Filter an array of objects: Extract all pod names in 'Running' status
kubectl get pods -o json | jq -r '.items[] | select(.status.phase=="Running") | .metadata.name'

# 4. Construct a new custom JSON object
aws ec2 describe-instances | jq '.Reservations[].Instances[] | {ID: .InstanceId, Type: .InstanceType, State: .State.Name}'
```

---

## 7. YAML Data Wrangling with `yq`

Kubernetes manifests, Docker Compose, and CI/CD pipelines use YAML. `yq` provides `jq`-like syntax for YAML files:

```bash
# 1. Read container image from a Kubernetes deployment manifest
yq '.spec.template.spec.containers[0].image' deployment.yaml

# 2. Update an image tag in-place (-i) during CI/CD release
yq -i '.spec.template.spec.containers[0].image = "my-app:v2.1.0"' deployment.yaml

# 3. Convert YAML to JSON seamlessly
yq -o=json deployment.yaml
```
