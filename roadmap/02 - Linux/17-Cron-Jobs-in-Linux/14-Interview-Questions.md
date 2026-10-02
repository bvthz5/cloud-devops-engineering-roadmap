# 14. Interview Questions and Answers

## Beginner Level

1. **What is cron?**
   Cron is a Linux time-based scheduler that runs commands or scripts automatically.

2. **What is a cron job?**
   A cron job is one scheduled task.

3. **What is crontab?**
   It is the table containing scheduled cron jobs for a user.

4. **How do you edit your crontab?**
   `crontab -e`

5. **How do you list cron jobs?**
   `crontab -l`

6. **How do you delete all jobs?**
   `crontab -r` (Use carefully).

7. **What are the five cron fields?**
   Minute, hour, day of month, month, day of week.

8. **What does `*` mean?**
   Every valid value for that field.

9. **What does `*/5` mean?**
   Every 5 units of that field.

10. **What does `1-5` mean in weekday position?**
    Monday through Friday.

## Intermediate Level

11. **Why should cron jobs use absolute paths?**
    Cron has a minimal environment and may not have the expected `PATH` or working directory.

12. **Why can a script work manually but fail in cron?**
    The environment, `PATH`, working directory, user context, permissions, or shell can differ.

13. **What does `2>&1` do?**
    It redirects stderr to the same destination currently used by stdout.

14. **How do you check whether cron is running?**
    `systemctl status cron` (Debian/Ubuntu) or `systemctl status crond` (RHEL/CentOS).

15. **Where can cron logs be found?**
    Depending on distro: `/var/log/syslog`, `/var/log/cron`, or `journalctl -u cron`.

16. **What is `@reboot`?**
    A cron shortcut for running a job when the cron service starts/at system boot.

17. **What is `/etc/crontab`?**
    A system-wide cron file that includes an explicit user field.

18. **What is `/etc/cron.d/`?**
    A drop-in directory for system-managed cron definitions.

19. **Cron vs anacron?**
    Cron is time-based and skips jobs if the system is powered off; anacron catches up on missed periodic jobs after boot.

20. **Cron vs systemd timer?**
    Systemd timers integrate deeply with systemd services, offering better logging and dependency management.

## Advanced Level

21. **What is a cron environment?**
    The minimal set of environment variables and execution context provided to cron jobs.

22. **How do you avoid overlapping cron jobs?**
    Use locking mechanisms such as `flock` (`/usr/bin/flock -n /tmp/lockfile command`).

23. **What is idempotency?**
    Running an operation repeatedly produces the exact same result without harmful duplicate side effects.

24. **What is a dangerous root cron configuration?**
    A privileged cron job executing code or scripts that an unprivileged user can modify.

25. **Why is logging important for cron?**
    Cron jobs run unattended in the background; logs provide execution proof and diagnostic error details.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - Scenario Based Questions](./13-Scenario-Based-Questions.md) | [README](./README.md) | [15 - MCQs](./15-MCQs.md) |
