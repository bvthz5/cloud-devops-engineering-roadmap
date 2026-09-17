# 31 — Production Scripting Checklist

## 25-Point Enterprise Production Readiness Checklist

### Code Health & Portability
- [ ] 1. Includes portable Shebang `#!/usr/bin/env bash` on Line 1.
- [ ] 2. Saved with Unix LF line endings (verified no CRLF `\r\n`).
- [ ] 3. Headers clearly define description, usage, author, version.
- [ ] 4. Strict mode `set -euo pipefail` enabled at header.
- [ ] 5. Passes `bash -n script.sh` without syntax warnings.
- [ ] 6. Passes `shellcheck script.sh` with zero high/medium issues.

### Safety & Error Control
- [ ] 7. All variable expansions double-quoted (`"$VAR"`).
- [ ] 8. Input arguments validated (`[[ $# -ge 1 ]]`).
- [ ] 9. Default parameters specified using `${1:-default}`.
- [ ] 10. Functions use `local` scope for internal variables.
- [ ] 11. Temporary files generated via `mktemp`.
- [ ] 12. Cleanup traps registered (`trap cleanup EXIT INT TERM`).
- [ ] 13. File locking implemented (`flock`) where race conditions could occur.
- [ ] 14. Configuration files written atomically (`mv`).

### I/O & Logging
- [ ] 15. Operational logs sent to `stderr` (`>&2`).
- [ ] 16. Logs contain ISO-8601 UTC timestamps.
- [ ] 17. Integrated with system logging (`logger`) where applicable.
- [ ] 18. Passwords/tokens hidden from process lists and output files.
- [ ] 19. Useful help output provided for `-h` / `--help` flags.

### Maintenance & DevOps
- [ ] 20. Code operations are idempotent.
- [ ] 21. Docker entrypoint scripts end with `exec "$@"`.
- [ ] 22. Absolute binary paths used in Cron scheduled tasks.
- [ ] 23. Pipeline errors caught with `set -o pipefail`.
- [ ] 24. Return values handled cleanly with explicit `exit N`.
- [ ] 25. Source material references preserved in `SOURCE.md`.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [30 - Command Cheat Sheet](./30-Complete-Command-Cheat-Sheet.md) | [README](./README.md) | [README (Index)](./README.md) |
