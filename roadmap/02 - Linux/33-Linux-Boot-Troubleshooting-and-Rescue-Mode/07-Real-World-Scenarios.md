# 07 — Real-World Boot Troubleshooting Scenarios

---

## Scenario 1: AWS EC2 Instance Unreachable After Editing `/etc/fstab`

### Incident Summary
An engineer adds a secondary EBS volume to `/etc/fstab`. The instance is rebooted.
SSH fails: `Connection timed out`. The AWS console shows: `2/2 checks failed: Instance reachability check failed`.

### Out-of-Band Cloud Recovery Runbook
1. Open the AWS EC2 Management Console.
2. Select the instance -> **Actions** -> **Monitor and troubleshoot** -> **EC2 Serial Console**.
3. View the live boot log. Notice:
   `Timed out waiting for device /dev/xvdf...`
   `You are in emergency mode. Enter root password or press Control-D to continue.`
4. Log into the emergency shell via the serial console.
5. Remount root: `mount -o remount,rw /`.
6. Add `nofail` to the offending line in `/etc/fstab`.
7. Reboot: `reboot`. The VM comes back online immediately!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Recovering from fstab Errors](./06-Recovering-from-fstab-and-Kernel-Panic.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
