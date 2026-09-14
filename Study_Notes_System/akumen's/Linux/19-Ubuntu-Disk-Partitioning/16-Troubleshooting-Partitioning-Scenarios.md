# 16. Troubleshooting Partitioning Scenarios

## Scenario 1: System drops into Emergency Maintenance Shell at boot
- **Cause:** Typo in `/etc/fstab` or missing disk UUID.
- **Resolution:**
  1. Boot into maintenance shell.
  2. Remount root as read-write: `mount -o remount,rw /`
  3. Edit `/etc/fstab` and correct typo or add `nofail` flag to non-critical mounts.

## Scenario 2: Partition table changes not recognized by kernel
- **Cause:** Disk is busy/in use.
- **Resolution:** Run `sudo partprobe` to force kernel to reload partition table without rebooting.
