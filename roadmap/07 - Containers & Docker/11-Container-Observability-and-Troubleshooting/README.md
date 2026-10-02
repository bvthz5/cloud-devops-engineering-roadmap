# 11 - Container Observability and Troubleshooting

Operating containers at enterprise scale requires rigorous telemetry, runtime debugging, and post-mortem crash forensics. SREs and Platform Engineers must dissect cgroups v2 resource metrics, interpret container exit codes (OOMKilled vs Segfault), configure non-blocking log rotation, and execute live troubleshooting on stripped Distroless containers using `nsenter` and eBPF tracing.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [cgroups v2 Resource Accounting & Metrics](./01-cgroups-v2-Resource-Accounting-and-Metrics.md) | Reading `/sys/fs/cgroup/`, Pressure Stall Information (PSI), CPU throttling metrics. |
| 02 | [Container Exit Codes & Crash Forensics](./02-Container-Exit-Codes-and-Crash-Forensics.md) | Deciphering exit codes: 0, 1, 125, 126, 127, 137 (OOM), 139 (Segfault), 143 (SIGTERM). |
| 03 | [Logging Drivers & Log Rotation Strategies](./03-Logging-Drivers-and-Log-Rotation-Strategies.md) | `json-file`, `journald`, syslog, fluentd, buffer modes (`mode=non-blocking`). |
| 04 | [cAdvisor & Prometheus Container Metrics](./04-Container-Health-Monitoring-with-cAdvisor-and-Prometheus.md) | Google cAdvisor architecture, tracking `container_cpu_cfs_throttled_periods_total`. |
| 05 | [Live Debugging with Ephemeral Containers & nsenter](./05-Live-Debugging-with-Ephemeral-Containers-and-nsenter.md) | Attaching to stripped distroless/scratch containers using host tools via `nsenter`. |
| 06 | [eBPF Container Tracing & Security Auditing](./06-eBPF-Based-Container-Tracing-and-Security-Auditing.md) | Zero-overhead tracing with eBPF, monitoring container syscalls with Falco and bpftrace. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Silent CPU throttling latency spike, blocking logging driver outage. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Diagnostic decision tree for OOMKilled vs Segfault, analyzing `dmesg -T` kernel logs. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on container metrics, PSI, and crash forensics. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Simulating OOMKill and analyzing dmesg; Lab 2: cgroups v2 PSI reader; Lab 3: Live nsenter. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for exit codes table, cgroups v2 metrics paths, and diagnostic CLI commands. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Podman, Buildah & Skopeo](../10-Podman-Buildah-and-Skopeo-Daemonless-Stack/README.md) | [README](./README.md) | [01 - cgroups v2 Resource Accounting](./01-cgroups-v2-Resource-Accounting-and-Metrics.md) |
