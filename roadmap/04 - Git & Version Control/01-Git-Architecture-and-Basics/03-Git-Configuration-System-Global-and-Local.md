# 03 - Git Configuration: System, Global, and Local

## 1. Configuration Hierarchy

Git resolves settings across three levels, where more specific levels override broader ones:

1. **Local (`--local`):** Specific to a single repository. Stored at `.git/config`.
2. **Global (`--global`):** User-specific across all repositories. Stored at `~/.gitconfig` or `~/.config/git/config`.
3. **System (`--system`):** Machine-wide for all users on the OS. Stored at `/etc/gitconfig`.

---

## 2. Essential Production Configurations

```bash
# Set author identity
git config --global user.name "Alice Developer"
git config --global user.email "alice@company.com"

# Set default initial branch name to main
git config --global init.defaultBranch main

# Set default text editor
git config --global core.editor "vim"

# Configure line ending normalization (CRLF vs LF)
# Linux / macOS:
git config --global core.autocrlf input
# Windows:
git config --global core.autocrlf true

# Configure default pull behavior to prevent unexpected merge commits
git config --global pull.rebase true

# Cache HTTPS credentials in memory for 1 hour (3600 seconds)
git config --global credential.helper "cache --timeout=3600"
```

---

## 3. Inspecting Active Settings
```bash
# List all active configs with origin file paths
git config --list --show-origin
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - The Three Trees](./02-The-Three-Trees-Working-Directory-Index-and-HEAD.md) | [README](./README.md) | [04 - Basic Workflow](./04-Basic-Workflow-Staging-Committing-and-Status.md) |
