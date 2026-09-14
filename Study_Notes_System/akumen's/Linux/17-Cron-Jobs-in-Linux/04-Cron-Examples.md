# 4. Cron Examples

## Common Schedule Patterns

### Every minute
```cron
* * * * * /path/to/script.sh
```

### Every hour
```cron
0 * * * * /path/to/script.sh
```
At minute 0 of every hour.

### Every day at midnight
```cron
0 0 * * * /path/to/script.sh
```

### Every day at 2:30 AM
```cron
30 2 * * * /path/to/script.sh
```

### Every Monday at 9 AM
```cron
0 9 * * 1 /path/to/script.sh
```

### First day of every month
```cron
0 0 1 * * /path/to/script.sh
```

### January 1 every year
```cron
0 0 1 1 * /path/to/script.sh
```

### Every 5 minutes
```cron
*/5 * * * * /path/to/script.sh
```

### Every 2 hours
```cron
0 */2 * * * /path/to/script.sh
```

### Every hour from 9 AM to 5 PM, Monday–Friday
```cron
0 9-17 * * 1-5 /path/to/script.sh
```

### Weekends at midnight
```cron
0 0 * * 0,6 /path/to/script.sh
```

### Python every 10 minutes
Prefer an explicit interpreter path:
```cron
*/10 * * * * /usr/bin/python3 /home/alice/scripts/check.py
```

### Backup every night at 2 AM
```cron
0 2 * * * /home/alice/scripts/backup.sh
```

## Important production rule

Use absolute paths:
```cron
0 2 * * * /home/alice/scripts/backup.sh
```
rather than:
```cron
0 2 * * * backup.sh
```

Cron has a limited environment and may not have the expected working directory or `PATH`.

## Scheduled service restart example
```cron
0 0 * * 0 /usr/bin/systemctl restart myapp
```
Use this only when automatic restarts are intentionally part of the design.

## Boot task example
```cron
@reboot /home/alice/scripts/startup.sh
```
`@reboot` runs the job when the cron service starts/at system startup according to the cron implementation. Do not assume it behaves exactly like a full dependency-aware service manager.
