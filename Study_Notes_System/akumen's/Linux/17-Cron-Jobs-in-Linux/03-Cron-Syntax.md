# 3. Cron Syntax — The Five Fields

## Basic format

A normal user crontab entry has:

```cron
* * * * * command
| | | | |
| | | | +-- Day of week (0 - 6) (Sunday=0 or 7)
| | | +---- Month (1 - 12)
| | +------ Day of month (1 - 31)
| +-------- Hour (0 - 23)
+---------- Minute (0 - 59)
```

Read it left to right:

Minute → Hour → Day of month → Month → Day of week → Command

## Allowed values
| Field | Values |
| --- | --- |
| Minute | 0–59 |
| Hour | 0–23 |
| Day of month | 1–31 |
| Month | 1–12 |
| Day of week | 0–6, with Sunday commonly 0 or 7 |

### Example:
```cron
30 2 * * * /path/to/script.sh
```

Meaning:
- Minute = 30
- Hour = 2
- Day = every day
- Month = every month
- Weekday = every weekday

So it runs every day at 2:30 AM.

## Special Characters

### The `*` wildcard
`*` means every valid value for that field. `* * * * *` means every minute.

### Lists: `,`
A comma selects multiple values. `0 9,12,18 * * *` runs at 09:00, 12:00, and 18:00.

### Ranges: `-`
A hyphen selects a range. `0 9-17 * * *` runs at minute 0 during hours 9 through 17.

### Steps: `/`
A slash means an interval. `*/15 * * * *` runs every 15 minutes. `0 */2 * * *` runs every 2 hours at minute 0.

## Combining syntax

You can combine lists, ranges, and steps.

Example:
```cron
*/15 9-17 * * 1-5 /path/to/script.sh
```

Meaning:
- Every 15 minutes
- During 9 AM–5 PM
- Monday–Friday

## A useful reading method

For every cron expression, ask:
1. What minute?
2. What hour?
3. Which day of the month?
4. Which month?
5. Which weekday?
6. What command runs?

## Important day-of-month/day-of-week note

Cron implementations can have special matching behavior when both day-of-month and day-of-week are restricted. Do not assume they always mean a simple logical AND across every implementation. Test complex schedules carefully.

## Source foundation

The supplied source provides the five-field syntax, allowed ranges, wildcard, list, range, and step examples.
