# 24 — Hands-On Labs 11 to 15: Infrastructure & Service Reliability

## Lab 11: CPU & RAM Utilization Watchdog

### Step-by-Step Instructions
1. Install `stress-ng` utility (`sudo apt install -y stress-ng`).
2. Run CPU stress test in background: `stress-ng --cpu 2 --timeout 30s &`.
3. Execute monitoring script and observe alert trigger when CPU usage spikes above 80%.

---

## Lab 12: Verifying Compressed Archive Integrity with MD5 Hashes

### Step-by-Step Instructions
1. Create sample directory `/tmp/lab12_data/` containing multiple files.
2. Run archival script to generate `/tmp/archive.tar.gz` and `/tmp/archive.tar.gz.md5`.
3. Verify MD5 hash using `md5sum -c /tmp/archive.tar.gz.md5`.

---

## Lab 13: Standardizing File Extensions in Bulk

### Step-by-Step Instructions
1. Create mock directory with uppercase extension files (`TOUCH FILE1.TXT FILE2.JPEG`).
2. Execute bulk renaming script.
3. Confirm files are renamed to lowercase without overwriting existing data.

---

## Lab 14: Measuring Network RTT Latency with `ping` & `awk`

### Step-by-Step Instructions
1. Execute multi-host network diagnostic script against internal and public DNS servers.
2. Verify output parses round-trip average time (RTT) correctly.

---

## Lab 15: Self-Healing Systemd Daemon Watchdog

### Step-by-Step Instructions
1. Stop Nginx service manually: `sudo systemctl stop nginx`.
2. Run watchdog script `sudo ./15_service_auto_restart.sh`.
3. Verify watchdog detects stoppage, restarts Nginx, and reports status recovery.
