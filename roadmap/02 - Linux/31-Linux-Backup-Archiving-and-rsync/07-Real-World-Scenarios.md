# 07 — Real-World Backup Production Scenarios

---

## Scenario 1: The Catastrophic `rsync --delete` Slash Accident

### Incident Summary
A junior engineer sets up a script to sync empty test data to the staging server.
Instead of `rsync -av /tmp/empty/ /var/data/`, they run:
`rsync -av --delete /tmp/empty/ /var/`
Within seconds, rsync wipes out `/var/log`, `/var/lib/docker`, and `/var/mail` because the source folder was empty!

### Prevention Best Practices
1. **Always test with `--dry-run` (`-n`) first!**
2. Use relative destination guards or write-protect destination roots.
3. Keep immutable cloud snapshots enabled.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Deduplicated Backups](./06-Deduplicated-and-Encrypted-Backups-Borg-Restic.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
