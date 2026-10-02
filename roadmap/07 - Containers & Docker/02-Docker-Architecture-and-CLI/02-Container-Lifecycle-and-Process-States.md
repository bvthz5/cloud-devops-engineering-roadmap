# 02 - Container Lifecycle and Process States

## 1. Container State Machine

```text
               docker create
  [ (Absent) ] ──────────────► [ Created ]
                                   │
                                   │ docker start / run
                                   ▼
          docker pause         [ Running ] ◄──────────────┐
       ┌───────────────────────┤         ├──────────────┐ │
       │                       └────┬────┘              │ │ docker unpause
       ▼                            │                   │ │
  [ Paused ] ───────────────────────┘                   │ │
                   docker unpause                       ▼ │
                                                [ Restarting ]
                   docker stop (SIGTERM -> SIGKILL)
                                    │
                                    ▼
                                [ Exited ]
                                    │
                                    │ docker rm
                                    ▼
                               [ (Absent) ]
```

---

## 2. Process Termination: `SIGTERM` vs `SIGKILL`

When running `docker stop <id>`:
1. Docker sends **`SIGTERM` (Signal 15)** to PID 1 inside the container, giving it a grace period (default: 10 seconds) to flush buffers and close database connections.
2. If the process does not terminate within the grace period, Docker forcefully sends **`SIGKILL` (Signal 9)**, terminating the process immediately.

```bash
# Graceful stop with 30-second timeout
docker stop -t 30 my_database_container

# Immediate brutal termination
docker kill my_container
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Docker Engine Architecture](./01-Docker-Engine-Architecture-and-Subsystems.md) | [README](./README.md) | [03 - Essential Docker CLI Commands](./03-Essential-Docker-CLI-Commands.md) |
