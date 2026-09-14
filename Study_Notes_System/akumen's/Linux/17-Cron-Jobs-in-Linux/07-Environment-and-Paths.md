# 7. Cron Environment and Paths

## The most common cron surprise

A command can work in your terminal but fail from cron.

Why? **Cron runs with a minimal environment.**

Your interactive shell may have:
- A larger `PATH`
- Custom aliases
- Exported variables
- A different working directory
- Shell initialization files (`.bashrc`, `.profile`)

Cron may not have them!

## Bad vs Better Examples

### Bad example:
```cron
0 2 * * * backup.sh
```
The command may not be found.

### Better example:
```cron
0 2 * * * /home/alice/scripts/backup.sh
```

## Interpreter paths

Bad:
```cron
*/10 * * * * python myscript.py
```

Better:
```cron
*/10 * * * * /usr/bin/python3 /home/alice/myscript.py
```

## Set PATH explicitly in crontab

A crontab can define variables at the top of the file:
```cron
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MAILTO=admin@example.com

0 2 * * * /home/alice/scripts/backup.sh
```

## Use absolute paths inside scripts too

Instead of depending on:
```bash
python3 app.py
```
consider explicitly selecting the interpreter and full file path.

Also avoid assuming the current working directory:
```bash
cat config.json
```
Prefer a known absolute path or calculate the script directory.

## Working directory

Cron does not necessarily start your script in the directory you expect.

This can break:
```bash
cat config.txt
```
if `config.txt` is assumed to be in the current directory.

Use:
```bash
cat /home/alice/app/config.txt
```
or deliberately change directory in the script (`cd /home/alice/app`).

## Environment variables

Bad assumption:
```bash
echo "$JAVA_HOME"
```
It may be empty under cron. Set required values explicitly or load them safely inside the script.

## Script permissions

Make executable when your cron entry executes the script directly:
```bash
chmod +x /home/alice/scripts/backup.sh
```

## Shebang

A script should identify its interpreter:
```bash
#!/bin/bash
```

Alternatively, invoke the interpreter explicitly in cron:
```cron
0 2 * * * /bin/bash /home/alice/scripts/backup.sh
```

## Debugging trick

Temporarily record the cron environment:
```cron
* * * * * env > /tmp/cron-env.txt
```
Then inspect:
```bash
cat /tmp/cron-env.txt
```
Remove the test job after debugging.

## Source foundation

The supplied source specifically warns about relative paths, executable permissions, and missing environment variables such as `PATH` and `JAVA_HOME`.
