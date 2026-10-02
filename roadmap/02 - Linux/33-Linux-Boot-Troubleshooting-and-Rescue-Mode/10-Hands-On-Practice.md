# 10 — Hands-On Practice Labs: Boot Troubleshooting

---

## Lab 1: Inspecting and Unpacking Initramfs

```bash
# Create temporary workspace
mkdir -p /tmp/initrd_inspect && cd /tmp/initrd_inspect

# Extract the current initramfs
unmkinitramfs /boot/initrd.img-$(uname -r) .

# Explore the minimal root environment
ls -la main/
ls -la main/lib/modules/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
