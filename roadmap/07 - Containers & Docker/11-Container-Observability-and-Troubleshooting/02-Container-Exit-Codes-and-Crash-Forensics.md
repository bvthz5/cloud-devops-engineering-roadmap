# 02 - Container Exit Codes and Crash Forensics

## 1. The Container Exit Code Rosetta Stone

When a container exits, its return code communicates the exact cause of failure:

| Exit Code | Meaning | Root Cause |
|---|---|---|
| **0** | Success | Clean, intentional process exit. |
| **1** | Application Error | General unhandled exception (Syntax error, missing env var). |
| **125** | Docker Run Error | Docker CLI error (e.g. invalid flag or image corrupt). |
| **126** | Permission Denied | Binary inside container is not executable (`chmod +x` missing). |
| **127** | Command Not Found | Executable specified in `ENTRYPOINT` does not exist in `$PATH`. |
| **137** | **SIGKILL / OOMKilled**| Exceeded memory limit (`128 + 9 = 137`) or `docker kill`. |
| **139** | **Segmentation Fault** | C/C++ memory corruption (`128 + 11 = 139`). |
| **143** | **SIGTERM** | Graceful termination signal sent by orchestrator (`128 + 15 = 143`). |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - cgroups v2 Resource Accounting and Metrics](./01-cgroups-v2-Resource-Accounting-and-Metrics.md) | [Index](../../../README.md) | [03 - Logging Drivers and Log Rotation Strategies →](./03-Logging-Drivers-and-Log-Rotation-Strategies.md) |
