# 27 — Production Incident Response Checklist

## Production Incident Response Checklist (20 Steps)

### Phase 1: Immediate Triage & Communication (0-5 Mins)
- [ ] 1. Acknowledge incident page in alert routing system (PagerDuty/Opsgenie).
- [ ] 2. Open emergency incident Slack/Teams channel and establish Incident Commander role.
- [ ] 3. Verify blast radius (Single node vs entire cluster vs customer-facing region).
- [ ] 4. Check status of global upstream cloud providers / load balancers.

### Phase 2: System Health Diagnostics (5-15 Mins)
- [ ] 5. Run `uptime` to check 1, 5, 15 minute load averages against core count.
- [ ] 6. Run `free -h` to check RAM availability and active swap paging.
- [ ] 7. Run `df -h` and `df -i` to check filesystem storage and inode capacity.
- [ ] 8. Check recent kernel errors (`dmesg -T --level=err | tail -n 30`).
- [ ] 9. Inspect application logs (`journalctl -u service -n 100 --no-pager`).
- [ ] 10. Check open network sockets (`ss -tulpn`).

### Phase 3: Stabilization & Mitigation (15-30 Mins)
- [ ] 11. If recent deployment occurred within 1 hour: **Trigger atomic rollback immediately**.
- [ ] 12. If disk is full due to unlinked open files: Truncate handles (`:> /proc/PID/fd/FD`).
- [ ] 13. If high CPU/RAM runaway process detected: Throttle (`renice`) or restart target daemon.
- [ ] 14. If port conflict detected: Kill blocking process (`fuser -k 80/tcp`).
- [ ] 15. If pod in `CrashLoopBackOff`: Check `kubectl logs --previous`.

### Phase 4: Verification & Post-Mortem (Post-Incident)
- [ ] 16. Verify system metrics (CPU, Memory, Disk, Latency) return to baseline.
- [ ] 17. Confirm customer-facing synthetic endpoint checks return HTTP 200 OK.
- [ ] 18. Preserve log archives and process memory dumps for root cause analysis.
- [ ] 19. Conduct Root Cause Analysis (RCA) blameless post-mortem meeting.
- [ ] 20. Implement automated monitoring alerts and regression test cases to prevent recurrence.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [26 - Scenario MCQs](./26-Scenario-Based-MCQs-and-Diagnostic-Quizzes.md) | [README](./README.md) | [28 - Quick Revision Notes](./28-Quick-Revision-Notes.md) |
