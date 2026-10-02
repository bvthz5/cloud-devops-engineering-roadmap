# 08 - Real-World Scenarios & DevOps Architectures

---

## 1. Enterprise Partitioning Strategy: Why Separate Partitions?

In a hobbyist or desktop installation, everything often resides on a single `/` root partition. In enterprise production servers, **putting everything on a single partition is a severe architectural anti-pattern**.

### What Happens When Everything is on `/`?
If a web application crashes and floods `/var/log/nginx/error.log` until the disk reaches 100% capacity:
- The Linux kernel cannot create lock files in `/run`.
- Databases like PostgreSQL or MySQL cannot commit transaction write-ahead logs (WAL).
- Nobody can SSH into the machine because the SSH daemon cannot allocate a pseudo-terminal or write session data!
- **The entire machine enters a deadlocked outage state.**

### Production Partition Layout (LVM Best Practice):

```
Physical / Virtual Disk
│
├── /boot       (1 - 2 GB, dedicated ext4 partition)
│               Prevents kernel upgrades from filling root.
│
├── / (Root)    (20 - 40 GB)
│               Houses /bin, /sbin, /etc, /usr.
│
├── /var        (50 - 500+ GB, or separate /var/log & /var/lib)
│               Isolates runaway application logs, database tables, and Docker images.
│
├── /tmp        (10 - 20 GB or RAM tmpfs, mounted with noexec,nosuid,nodev)
│               Prevents malicious scripts from running out of /tmp.
│
└── /data       (Dedicated EBS / SAN volume)
                Application code, object storage caches, microservice state.
```

---

## 2. Docker & Container Storage Under the Hood

When you run containers on a host, Docker doesn't use magical isolation—it uses standard Linux directory structures, primarily backed by **OverlayFS**.

### Where Docker Stores Data on the Host:
- **Default root directory:** `/var/lib/docker`
- **Container image layers & copy-on-write:** `/var/lib/docker/overlay2`
- **Named Volumes:** `/var/lib/docker/volumes/<volume_name>/_data`
- **Container logs:** `/var/lib/docker/containers/<container_id>/<container_id>-json.log`

```
Host Filesystem:
/var/lib/docker/
├── overlay2/                  <-- Layered read-only image layers + write layer
│   ├── abc123def/diff
│   └── 456xyz789/diff
└── volumes/
    └── postgres_data/
        └── _data/             <-- PostgreSQL writes directly here
            ├── base/
            └── pg_wal/
```

> [!TIP]
> **Production Fix for Runaway Docker Disks:**
> If `/var/lib/docker` runs out of space:
> ```bash
> # Remove unused containers, networks, images, and build caches:
> $ docker system prune -a --volumes
> ```

---

## 3. AWS EC2: Attaching & Mounting an EBS Volume Without Rebooting

### The Scenario:
You provision an AWS EC2 instance. The root drive (`/dev/xvda` or `/dev/nvme0n1`) is 20 GB. You attach a new 100 GB EBS volume for database data and want it mounted permanently at `/data`.

### Step-by-Step Production Procedure:

```bash
# 1. Inspect existing disks and detect the new raw volume:
$ lsblk
NAME         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
nvme0n1      259:0    0   20G  0 disk 
└─nvme0n1p1  259:1    0   20G  0 part /
nvme1n1      259:2    0  100G  0 disk   <-- Newly attached raw disk!

# 2. Verify whether the new disk has an existing filesystem:
$ sudo file -s /dev/nvme1n1
/dev/nvme1n1: data   # ("data" means raw, unformatted disk)

# 3. Format the volume with modern high-performance XFS:
$ sudo mkfs -t xfs /dev/nvme1n1

# 4. Create the target mount directory:
$ sudo mkdir -p /data

# 5. Get the unique UUID of the new filesystem:
$ sudo blkid /dev/nvme1n1
/dev/nvme1n1: UUID="7a8f1b2c-3d4e-5f6a-7b8c-9d0e1f2a3b4c" TYPE="xfs"

# 6. Add to /etc/fstab using UUID with nofail:
$ echo 'UUID=7a8f1b2c-3d4e-5f6a-7b8c-9d0e1f2a3b4c /data xfs defaults,nofail 0 2' | sudo tee -a /etc/fstab

# 7. Test the mount without rebooting:
$ sudo mount -a

# 8. Verify the mount succeeded:
$ df -h /data
Filesystem      Size  Used Avail Use% Mounted on
/dev/nvme1n1    100G  746M  100G   1% /data
```

---

## 4. Hardening Filesystems: Security Mount Flags

In security compliance frameworks (CIS Benchmark, NIST, SOC 2), securing `/tmp` and `/dev/shm` is mandatory.

Hackers often download and execute malware in `/tmp` because `/tmp` has world-writable permissions (`chmod 1777`).

### Hardening Options in `/etc/fstab`:
```
# <file system> <mount point> <type>  <options>                   <dump> <pass>
tmpfs           /tmp          tmpfs   defaults,noexec,nosuid,nodev 0      0
tmpfs           /dev/shm      tmpfs   defaults,noexec,nosuid,nodev 0      0
```

- **`noexec`:** Disallows execution of any binary or script located on this partition. (Even if a hacker drops `malware.sh` in `/tmp` and runs `chmod +x`, execution is blocked by the kernel).
- **`nosuid`:** Disallows SUID privilege escalation flags.
- **`nodev`:** Prevents creation of character or block device nodes.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Practical Commands](./07-Practical-Commands.md) | [README](./README.md) | [09 - Troubleshooting](./09-Troubleshooting.md) |
