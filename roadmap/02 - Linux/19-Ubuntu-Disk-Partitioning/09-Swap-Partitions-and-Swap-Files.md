# 9. Swap Partitions vs Swap Files

## What is Swap?
Swap space acts as virtual memory when physical RAM is exhausted. Idle memory pages are paged out from RAM to Swap.

## Swap Partition vs Swap File
- **Swap Partition:** Dedicated raw disk partition formatted as swap.
- **Swap File:** Dynamic file inside an existing filesystem (Ubuntu default since 17.04). Performance difference is negligible on modern kernels.

## Managing Swap Files

### 1. Create a 4 GB Swap File
```bash
# Allocate 4GB file using fallocate
sudo fallocate -l 4G /swapfile

# Set strict permissions (read/write root only)
sudo chmod 600 /swapfile

# Format as Swap
sudo mkswap /swapfile

# Activate Swap file
sudo swapon /swapfile

# Verify swap status
sudo swapon --show
free -h
```

### 2. Add Swap File to `/etc/fstab`
```config
/swapfile  none  swap  sw  0  0
```

## Tuning Swappiness (`sysctl`)
`vm.swappiness` (0 - 100) controls kernel aggressiveness in swapping memory pages:
- Default: `60`
- Recommended for Production Servers / DBs: `10` – `20`

Set runtime swappiness:
```bash
sudo sysctl vm.swappiness=10
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Etc Fstab and UUID Mounting](./08-Etc-Fstab-and-UUID-Mounting.md) | [README](./README.md) | [10 - LVM Physical Volumes Volume Groups Logical Volumes](./10-LVM-Physical-Volumes-Volume-Groups-Logical-Volumes.md) |
