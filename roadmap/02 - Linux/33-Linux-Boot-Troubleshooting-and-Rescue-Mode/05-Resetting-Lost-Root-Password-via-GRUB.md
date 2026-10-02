# 05 — Resetting Lost Root Password via GRUB

When root credentials are lost and sudo access is unavailable, you can reset the password directly via console access.

---

## Method A: RHEL / Rocky / CentOS (`rd.break`)

1. Reboot the server. At the GRUB boot menu, select the default kernel and press **`e`**.
2. Locate the line starting with `linux` or `linux16`.
3. Append `rd.break` to the end of the line:
   ```text
   linux ($root)/vmlinuz-... root=/dev/mapper/rl-root ro rd.break
   ```
4. Press **`Ctrl + X`** to boot. You will drop into the initramfs `switch_root:/#` prompt.
5. Remount `/sysroot` in read-write mode:
   ```bash
   mount -o remount,rw /sysroot
   ```
6. Chroot into the real system:
   ```bash
   chroot /sysroot
   ```
7. Change the root password:
   ```bash
   passwd root
   ```
8. **CRITICAL (SELinux):** Instruct SELinux to relabel the modified shadow file upon reboot:
   ```bash
   touch /.autorelabel
   ```
9. Exit and reboot:
   ```bash
   exit
   exit
   ```

---

## Method B: Ubuntu / Debian (`init=/bin/bash`)

1. At the GRUB boot menu, select the kernel and press **`e`**.
2. Find the line starting with `linux`. Replace `ro quiet splash` with:
   ```text
   rw init=/bin/bash
   ```
3. Press **`Ctrl + X`**. You will boot directly into a root `#` bash prompt.
4. Set password:
   ```bash
   passwd root
   ```
5. Reboot:
   ```bash
   exec /sbin/init
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Emergency vs Rescue Mode](./04-Emergency-Mode-vs-Rescue-Mode.md) | [README](./README.md) | [06 - Recovering from fstab Errors](./06-Recovering-from-fstab-and-Kernel-Panic.md) |
