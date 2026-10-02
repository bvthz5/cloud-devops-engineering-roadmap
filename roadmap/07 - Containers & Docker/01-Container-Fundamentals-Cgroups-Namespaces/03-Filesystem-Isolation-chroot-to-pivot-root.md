# 03 - Filesystem Isolation: chroot to pivot_root

## 1. The Vulnerability of `chroot`

Historically, Unix used `chroot` to change the root filesystem directory for a process. However, **`chroot` is NOT a security boundary**:
- A root process inside a `chroot` jail can easily escape by opening a file descriptor to the outside, creating a subdirectory, and calling `chroot` again:

```c
// Classic chroot breakout exploit
int fd = open(".", O_RDONLY);
chroot("empty_dir");
fchdir(fd);
chroot("../../../../../../../"); // Escaped back to real host root!
```

---

## 2. Atomic Root Swapping with `pivot_root`

Containers use **`pivot_root`** within an isolated Mount Namespace (`CLONE_NEWNS`):
1. Creates a new mount namespace.
2. Mounts the container rootfs.
3. Moves the current host root filesystem to an unlinked directory (e.g., `/old_root`).
4. Atomically swaps the new rootfs to `/`.
5. Unmounts `/old_root` with `umount -l /old_root`.
6. Once unmounted, it is **physically impossible** for processes inside the container to reference host inodes!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Control Groups cgroups v1 vs cgroups v2](./02-Control-Groups-cgroups-v1-vs-cgroups-v2.md) | [Index](../../../README.md) | [04 - Union Filesystems and OverlayFS Internals →](./04-Union-Filesystems-and-OverlayFS-Internals.md) |
