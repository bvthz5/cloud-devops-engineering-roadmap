# 2. Crontab and Essential Commands

## What is crontab?

`crontab` means cron table.

It stores scheduled jobs for a user.

Normally, you manage your user's crontab with the `crontab` command rather than editing the underlying spool file directly.

## Essential commands

### Edit
```bash
crontab -e
```
Opens your crontab in an editor.

### List
```bash
crontab -l
```
Displays your scheduled jobs.

### Remove all jobs
```bash
crontab -r
```
**Danger**: this removes all jobs for that user's crontab. Always verify before using it.

### Root editing another user's crontab
```bash
sudo crontab -u alice -e
```
This requires appropriate privileges.

## Basic workflow
```text
crontab -e
     |
     v
Add schedule + command
     |
     v
Save and exit
     |
     v
Cron reads the new schedule
     |
     v
Job runs at matching times
```

## Comments

Use `#` to temporarily disable a line:

```cron
# 0 2 * * * /home/alice/scripts/backup.sh
```

Cron ignores the commented line. This is safer than deleting a job when you may need it later.

## Important warning

Do not casually use:
```bash
crontab -r
```

If you only want to remove one job, use:
```bash
crontab -e
```
and delete or comment out that specific line.

## First-time editor selection

On some systems, the first `crontab -e` may ask which editor to use. For beginners, `nano` is usually easier.

## Source foundation

The supplied source covers `crontab -e`, `crontab -l`, `crontab -r`, comments for temporarily disabling jobs, and editing another user's crontab with root privileges.
