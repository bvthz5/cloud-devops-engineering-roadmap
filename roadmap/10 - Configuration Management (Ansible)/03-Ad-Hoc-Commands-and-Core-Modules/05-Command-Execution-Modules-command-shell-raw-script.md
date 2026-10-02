# 05 - Command Execution Modules: command vs shell vs raw vs script

| Module | Features | Shell Piping/Redirects (`\|`, `>`) | Requires Python? |
|---|---|---|---|
| `command` | Safe, default module | **NO** | YES |
| `shell` | Runs via `/bin/sh` | **YES** | YES |
| `raw` | Low-level SSH command | **YES** | **NO** (Use to bootstrap Python) |
| `script` | Runs local script on target | **YES** | YES |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - User and Security Modules user group authorized_key copy](./04-User-and-Security-Modules-user-group-authorized_key-copy.md) | [Index](../../../README.md) | [06 - Network and Utility Modules ping uri get_url stat →](./06-Network-and-Utility-Modules-ping-uri-get_url-stat.md) |
