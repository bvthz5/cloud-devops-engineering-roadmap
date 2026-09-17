# 13 — Production Deployment Failures and Rollback

## 1. Scenario
A automated deployment deploys broken code version `v2.4.1`, causing HTTP 500 errors across all production nodes.

```text
Scenario
   ↓
Symptoms: HTTP 500 Internal Server Errors, database migration failure, app crash loop on start
   ↓
What could cause it? Syntax error, missing env var, failed DB migration
   ↓
Fix / mitigate: Atomic symlink rollback to previous release directory, reload web server
```

## 2. Atomic Symlink Deployment Architecture

```
/var/www/my-app/
├── current -> releases/v2.4.0   (Symlink pointed to active release)
└── releases/
    ├── v2.3.9
    ├── v2.4.0  (Stable)
    └── v2.4.1  (Broken)
```

## 3. Performing Instant Atomic Rollback

```bash
# Atomically point 'current' symlink back to stable v2.4.0
ln -snf /var/www/my-app/releases/v2.4.0 /var/www/my-app/current

# Reload application daemon
systemctl reload nginx
systemctl restart my-app
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - DNS Resolution Failures](./12-DNS-Resolution-Failures-and-Diagnostic-Tools.md) | [README](./README.md) | [14 - Docker Container Troubleshooting](./14-Docker-Container-Troubleshooting.md) |
