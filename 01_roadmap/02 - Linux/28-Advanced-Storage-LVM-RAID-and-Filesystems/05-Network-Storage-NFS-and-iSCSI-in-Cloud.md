# 05 — Network Storage: NFS and Cloud Mounts

Shared network filesystems allow multiple web nodes or Kubernetes pods to access shared assets, media uploads, and configuration files simultaneously.

---

## 1. NFSv4 Server and Client Setup

### On NFS Server:
```bash
# Install NFS kernel server
sudo apt-get install -y nfs-kernel-server

# Create export directory
sudo mkdir -p /srv/nfs/shared
sudo chown -R nobody:nogroup /srv/nfs/shared

# Configure export rules in /etc/exports
echo "/srv/nfs/shared 10.0.1.0/24(rw,sync,no_subtree_check,no_root_squash)" | sudo tee -a /etc/exports

# Apply exports
sudo exportfs -rav
```

### On NFS Client (Web / App Servers):
```bash
sudo apt-get install -y nfs-common
sudo mkdir -p /mnt/shared

# Mount with production-optimized NFSv4 options
sudo mount -t nfs -o proto=tcp,port=2049,nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600 10.0.1.10:/srv/nfs/shared /mnt/shared
```

---

## 2. Production `/etc/fstab` Persistent Mounts

Never mount cloud volumes with brittle device names like `/dev/sdb` (device letters can shift across reboots!). Always mount via **UUID**:

```text
# /etc/fstab entry
UUID=3f1b4a22-9a3d-4e9b-810a-313210190123 /data ext4 defaults,noatime,nofail 0 2
```
- **`noatime`:** Disables writing access timestamps every time a file is read, boosting I/O performance by 20–30%.
- **`nofail`:** Critical in cloud environments. If the secondary disk fails to attach, the VM will still boot normally instead of halting into an emergency recovery shell.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Filesystem Internals ext4 XFS and Btrfs ZFS](./04-Filesystem-Internals-ext4-XFS-and-Btrfs-ZFS.md) | [Index](../../../README.md) | [06 - Online Filesystem Expansion and Maintenance →](./06-Online-Filesystem-Expansion-and-Maintenance.md) |
