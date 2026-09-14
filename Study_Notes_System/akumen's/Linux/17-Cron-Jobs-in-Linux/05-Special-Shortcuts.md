# 5. Cron Shortcut Strings

Cron supports readable aliases.

| Shortcut | Typical equivalent | Meaning |
| --- | --- | --- |
| `@reboot` | — | Run once at startup |
| `@yearly` / `@annually` | `0 0 1 1 *` | Once a year |
| `@monthly` | `0 0 1 * *` | Once a month |
| `@weekly` | `0 0 * * 0` | Once a week |
| `@daily` / `@midnight` | `0 0 * * *` | Once a day |
| `@hourly` | `0 * * * *` | Once an hour |

## Examples
```cron
@daily /home/alice/scripts/backup.sh
@reboot /home/alice/scripts/start-services.sh
```

## When aliases are useful

Use them when the schedule is naturally described by the alias.

For more specific schedules, use the five-field format.

Example:
```cron
30 2 * * * /path/to/script.sh
```
is clearer than trying to express a 2:30 AM schedule with a shortcut that does not exist.

## Important distinction

`@reboot` is not the same as a systemd service.

If an application has:
- Dependencies
- Ordering requirements
- Restart policies
- Readiness requirements
- Detailed service logging

then a service manager (like `systemd`) is a better fit.
