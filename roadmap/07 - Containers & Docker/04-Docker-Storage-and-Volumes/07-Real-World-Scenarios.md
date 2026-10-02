# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Host Permission Overwrite Outage

### Context & Incident
A developer mounted their local source directory into a Node.js container:
`docker run -v $(pwd):/app -w /app mynode npm install`
After execution, the developer could no longer edit local files, and Git operations failed with `Permission denied`.

### Root Cause
By default, the container executed as `root` (UID 0). The `npm install` command created the `node_modules` directory owned by `root:root` on the host filesystem!

### Solution: Match Host User ID
```bash
docker run --user "$(id -u):$(id -g)" -v "$(pwd)":/app -w /app mynode npm install
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Storage Optimization & Cleanup](./06-Storage-Optimization-and-Dangling-Cleanup.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
