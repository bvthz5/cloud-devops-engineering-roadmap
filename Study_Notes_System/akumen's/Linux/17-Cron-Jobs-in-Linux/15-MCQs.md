# 15. Multiple Choice Questions

1. Which command edits a user's crontab?
   - A. `cron -e`
   - B. `crontab -e`
   - C. `cronjob -e`
   - D. `schedule -e`
   **Answer: B**

2. Which field comes first in cron syntax?
   - A. Hour
   - B. Month
   - C. Minute
   - D. Weekday
   **Answer: C**

3. What does `*/10` usually mean?
   - A. Ten times immediately
   - B. Every 10 units
   - C. Only at 10
   - D. Ten days
   **Answer: B**

4. What does `2>&1` do?
   - A. Deletes stderr
   - B. Sends stdout to stderr
   - C. Sends stderr to stdout's current destination
   - D. Runs as root
   **Answer: C**

5. Which command lists the current user's cron jobs?
   - A. `crontab -l`
   - B. `crontab -e`
   - C. `cron -l`
   - D. `systemctl cron`
   **Answer: A**

6. Which is safest for a script path in cron?
   - A. `backup.sh`
   - B. `./backup.sh`
   - C. `~/backup.sh`
   - D. `/usr/local/bin/backup.sh`
   **Answer: D**

7. What does `@daily` represent?
   - A. Every hour
   - B. Once daily (`0 0 * * *`)
   - C. Every weekday
   - D. At system startup
   **Answer: B**

8. Which directory commonly contains daily cron scripts?
   - A. `/var/daily`
   - B. `/etc/cron.daily`
   - C. `/cron/daily`
   - D. `/usr/cron.daily`
   **Answer: B**

9. Which alternative is designed to handle periodic jobs on machines that may be powered off?
   - A. `anacron`
   - B. `grep`
   - C. `sed`
   - D. `curl`
   **Answer: A**

10. Which scheduler integrates directly with systemd services?
   - A. `cron`
   - B. `systemd timers`
   - C. `awk`
   - D. `wget`
   **Answer: B**

11. What does `crontab -r` do?
   - A. Restart cron
   - B. Remove all jobs in the user's crontab
   - C. Read the crontab
   - D. Run all jobs
   **Answer: B**

12. Which is a good production practice?
   - A. Hide all errors
   - B. Use relative paths
   - C. Log important jobs
   - D. Run everything as root
   **Answer: C**

13. What is a major security risk with privileged cron jobs?
   - A. They use timestamps
   - B. An unprivileged user can modify executable content used by the privileged job
   - C. They use five fields
   - D. They create logs
   **Answer: B**

14. Which command is useful for checking the service on Ubuntu/Debian?
   - A. `systemctl status cron`
   - B. `cron status`
   - C. `service list cron`
   - D. `crontab status`
   **Answer: A**

15. Why can a command work in a terminal but fail in cron?
   - A. Cron changes Linux kernel
   - B. Cron may use a different/minimal environment
   - C. Cron cannot run shell commands
   - D. Cron only runs as root
   **Answer: B**
