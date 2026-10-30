# 02 — Command Syntax, Paths, and Directory Navigation

---

## 1. Anatomy of a Command Line

Every shell command adheres to a standardized structural grammar:

```text
       command       options/flags           arguments (operands)
    ┌───────────┐  ┌──────────────┐  ┌───────────────────────────────────┐
    │  tar      │  │  -c -z -f    │  │  backup.tar.gz   /var/www/html/   │
    └───────────┘  └──────────────┘  └───────────────────────────────────┘
```

### Components of Command Syntax:
1. **Command:** The executable binary, shell built-in, or script to invoke (e.g., `ls`, `grep`, `tar`, `kubectl`).
2. **Options / Flags:** Modifiers that alter the execution behavior of the command:
   - **Short Options:** Single hyphen followed by a single character (e.g., `-l`, `-a`, `-h`).
   - **Combined Short Options:** Multiple single-character flags grouped behind a single hyphen: `-lah` is identical to `-l -a -h`.
   - **Long Options (Flags):** Double hyphens followed by a descriptive word (e.g., `--all`, `--human-readable`, `--output=json`).
3. **Arguments (Operands):** The target entities upon which the command acts (files, directories, IP addresses, URLs, strings).

---

## 2. Paths: Absolute vs Relative

In Linux, the filesystem is a single inverted tree rooted at **`/`**. Every file and directory is addressed via a **Path**.

```text
Filesystem Tree:
/ (Root)
├── etc/
│    └── nginx/
│         └── nginx.conf  <-- Absolute Path: /etc/nginx/nginx.conf
└── var/
     └── log/            <-- Current Working Directory: /var/log/
          └── sys.log
```

### Absolute Path
- **Definition:** Specifies the exact location of a file starting from the top-level **Root Directory (`/`)**.
- **Rule:** **Always starts with a leading forward slash `/`**.
- **Invariant:** Resolves to the identical file regardless of which directory the user is currently working in.
- **Examples:**
  - `/etc/hosts`
  - `/var/log/nginx/access.log`
  - `/home/ubuntu/.ssh/authorized_keys`

### Relative Path
- **Definition:** Specifies the location of a file **relative to your Current Working Directory (`pwd`)**.
- **Rule:** **Never starts with a leading forward slash `/`**.
- **Context-dependent:** Resolves to different locations depending on where you are.
- **Examples:**
  - If current working directory is `/var`:
    - `log/nginx/access.log` resolves to `/var/log/nginx/access.log`.
  - If current working directory is `/home/ubuntu`:
    - `projects/app/main.go` resolves to `/home/ubuntu/projects/app/main.go`.

---

## 3. Special Path Symbols: `.`, `..`, and `~`

Unix defines three essential shorthand path abstractions:

| Symbol | Representation | Meaning | Example Usage |
| :--- | :--- | :--- | :--- |
| **`.`** | Current Directory | The directory you are currently standing in. | `./deploy.sh` (executes script in current folder) |
| **`..`** | Parent Directory | The immediate directory one level up in the hierarchy. | `cd ..` or `cat ../config.yaml` |
| **`~`** | User Home Directory| Expands to the active user's `$HOME` path. | `cd ~` or `ls ~/.ssh/` |
| **`~user`**| Specific User Home | Expands to another specific user's home directory. | `cat ~postgres/data/pg_hba.conf` |
| **`-`** | Previous Directory | The directory you were in immediately prior to last `cd`. | `cd -` (toggles back to previous location) |

```text
Navigating with Relative Paths:
If standing in: /var/log/nginx/
- cd .   ──► Stays in /var/log/nginx/
- cd ..  ──► Moves up to /var/log/
- cd ../.. ──► Moves up two levels to /var/
- cd ../../etc/ ──► Moves up to /var/, then enters /etc/ (Path: /etc/)
```

---

## 4. Essential Directory Navigation Commands

```bash
# 1. Print the current Working Directory (Absolute Path)
pwd

# 2. Change directory to root
cd /

# 3. Change directory to user home directory
cd
# or:
cd ~

# 4. Jump back to the previously visited directory (directory toggle)
cd -

# 5. Move up two directory levels
cd ../..

# 6. Directory Stack Navigation (Push and Pop)
# Save current directory and jump to /var/log
pushd /var/log

# Return to previous saved directory from stack
popd
```

---

## 5. DevOps Best Practice: Relative vs Absolute Paths in Automation

> **CRITICAL SRE RULE:**  
> In bash scripts, Dockerfiles, and CI/CD pipelines, **NEVER use relative paths for critical system files or operations**.
>
> **Dangerous:**
> ```bash
> # If script is executed from the wrong directory, it deletes local code!
> rm -rf data/cache/*
> ```
>
> **Safe & Robust:**
> ```bash
> SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
> rm -rf "${SCRIPT_DIR}/../data/cache"/*
> ```
> Or use absolute system paths:
> ```bash
> rm -rf /var/cache/app/*
> ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Terminal vs Shell and Shell Flavors](./01-Terminal-vs-Shell-and-Shell-Flavors.md) | [Index](../../../README.md) | [03 - PATH Command Resolution Builtins and Aliases →](./03-PATH-Command-Resolution-Builtins-and-Aliases.md) |
