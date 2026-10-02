# 04 - Union Filesystems and OverlayFS Internals

## 1. Copy-on-Write (CoW) Mechanics

Container images are composed of immutable, read-only layers stacked on top of each other. When a container runs, Docker attaches a single thin, read-write layer on top using **OverlayFS** (`overlay2`).

```text
[ /merged Directory ] (What the container sees as its root filesystem /)
          ▲
          │ (Union Mount)
  ┌───────┴──────────────────────────────┐
  │ [ upperdir ] (Container R/W Layer)   │  <-- Newly created/modified files
  ├──────────────────────────────────────┤
  │ [ workdir ]  (Kernel Scratch Space)  │
  ├──────────────────────────────────────┤
  │ [ lowerdir 3 ] (Read-Only Image Layer)│ <-- e.g., RUN npm install
  │ [ lowerdir 2 ] (Read-Only Image Layer)│ <-- e.g., RUN apt-get install python
  │ [ lowerdir 1 ] (Base OS Layer)       │ <-- e.g., ubuntu:22.04 base
  └──────────────────────────────────────┘
```

---

## 2. File Operations in OverlayFS

1. **Reading a file**: Kernel searches from top to bottom (`upperdir` ➔ `lowerdir 3` ➔ `lowerdir 2`...). First copy found is returned.
2. **Modifying an existing file**: The file is copied from the read-only `lowerdir` into the writable `upperdir` (**Copy-on-Write**). The modification occurs in `upperdir`, masking the original file.
3. **Deleting a file**: OverlayFS creates a special **Whiteout character device** (`0:0` device node) in `upperdir`. The file still exists in the lower layer, but is invisible to the container!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Filesystem Isolation chroot to pivot root](./03-Filesystem-Isolation-chroot-to-pivot-root.md) | [Index](../../../README.md) | [05 - Building a Container from Scratch in Bash →](./05-Building-a-Container-from-Scratch-in-Bash.md) |
