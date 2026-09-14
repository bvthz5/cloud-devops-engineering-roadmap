# 12. Hands-On Practice

Practice in a VM or non-production machine.

## Lab 1 — Your first cron job

### Objective
Write the current date to a file every minute.

### Steps
1. Create the directory:
   ```bash
   mkdir -p ~/scripts
   ```
2. Create the script:
   ```bash
   nano ~/scripts/log-date.sh
   ```
   Content:
   ```bash
   #!/bin/bash
   echo "Current time: $(date)" >> ~/scripts/date-log.txt
   ```
3. Make it executable:
   ```bash
   chmod +x ~/scripts/log-date.sh
   ```
4. Edit crontab:
   ```bash
   crontab -e
   ```
5. Add:
   ```cron
   * * * * * /home/YOUR_USERNAME/scripts/log-date.sh
   ```
6. Watch output:
   ```bash
   tail -f ~/scripts/date-log.txt
   ```
7. Stop it: `crontab -e` and remove the line.

## Lab 2 — Cron logging

1. Create script:
   ```bash
   nano ~/scripts/test-log.sh
   ```
   Content:
   ```bash
   #!/bin/bash
   echo "SUCCESS $(date)"
   echo "ERROR-TEST $(date)" >&2
   ```
2. Make executable: `chmod +x ~/scripts/test-log.sh`
3. Schedule with redirection:
   ```cron
   * * * * * /home/YOUR_USERNAME/scripts/test-log.sh >> /tmp/test-cron.log 2>&1
   ```
4. Check output: `cat /tmp/test-cron.log`

## Lab 3 — Environment difference
1. Add temporarily to crontab:
   ```cron
   * * * * * env > /tmp/cron-env.txt
   ```
2. Inspect: `cat /tmp/cron-env.txt`
3. Compare with your shell environment: `env`
4. Remove the test job afterward.

## Lab 4 — Python cron job
1. Create script: `nano ~/scripts/check.py`
   ```python
   from datetime import datetime
   with open("/tmp/python-cron.log", "a") as f:
       f.write(f"Python ran at {datetime.now()}\n")
   ```
2. Find Python path: `command -v python3`
3. Schedule every 5 minutes:
   ```cron
   */5 * * * * /usr/bin/python3 /home/YOUR_USERNAME/scripts/check.py
   ```

## Lab 5 — Troubleshoot a broken cron job
1. Create a broken entry:
   ```cron
   * * * * * nonexistent-command >> /tmp/broken.log 2>&1
   ```
2. Inspect log: `cat /tmp/broken.log`
3. Fix or remove the job.

## Lab 6 — Prevent overlapping jobs
1. Create script: `nano ~/scripts/lock-test.sh`
   ```bash
   #!/bin/bash
   echo "start $(date)" >> /tmp/lock-test.log
   sleep 90
   echo "end $(date)" >> /tmp/lock-test.log
   ```
2. Make executable: `chmod +x ~/scripts/lock-test.sh`
3. Schedule every minute using a lock:
   ```cron
   * * * * * /usr/bin/flock -n /tmp/lock-test.lock /home/YOUR_USERNAME/scripts/lock-test.sh
   ```
4. Observe that `flock` prevents multiple simultaneous executions.

## Lab 7 — System-wide investigation
1. Inspect directories:
   ```bash
   ls -ld /etc/cron.*
   ls -l /etc/cron.d/
   ```
2. Read `/etc/crontab`: `cat /etc/crontab`
