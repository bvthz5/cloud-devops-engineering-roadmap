# 25 — Hands-On Labs 16 to 20: CI/CD and Container Operations

## Lab 16: Building Automated Git Pull & Reload Pipeline

### Step-by-Step Instructions
1. Initialize local Git repository in `/var/www/test-app`.
2. Configure local origin tracking branch.
3. Execute deployment script and verify commit hash comparison prevents unnecessary reloads.

---

## Lab 17: Synchronizing Directories with `rsync --checksum`

### Step-by-Step Instructions
1. Create primary and secondary folders.
2. Add files to primary folder and execute `17_directory_sync.sh`.
3. Modify a file in primary folder and verify `rsync --checksum` updates only changed files.

---

## Lab 18: Gathering System Specifications for Hardware Audits

### Step-by-Step Instructions
1. Execute `18_system_info_report.sh`.
2. Inspect output report at `/tmp/system_summary.txt`.
3. Verify accurate extraction of CPU cores, memory size, and kernel version.

---

## Lab 19: Monitoring Node.js / Python Process Life Cycles

### Step-by-Step Instructions
1. Write sample long-running Node.js background process.
2. Kill background process manually via `kill -9 <PID>`.
3. Run process watchdog script to verify automatic PID file update and application restart.

---

## Lab 20: Automating Docker Host Storage Reclamation

### Step-by-Step Instructions
1. Build temporary Docker container images: `docker run --name temp_test alpine echo "test"`.
2. Stop container.
3. Run `20_docker_cleanup.sh` and observe disk space reclaimed via `docker system df`.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [24 - Hands-On Labs 11-15](./24-Hands-On-Labs-11-to-15-Network-and-DB.md) | [README](./README.md) | [26 - Troubleshooting Scenarios](./26-Troubleshooting-Scenarios-and-Common-Pitfalls.md) |
