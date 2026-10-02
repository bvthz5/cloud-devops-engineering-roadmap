# 07 - Third-Party Apps & Services: `/opt` vs `/srv`

Both `/opt` and `/srv` store application-related assets, but they serve distinct purposes according to the Filesystem Hierarchy Standard (FHS).

---

## 📦 1. The `/opt` Directory (Optional Software Packages)

`/opt` (short for **Optional Application Software Packages**) is reserved for self-contained, 3rd-party commercial or vendor software that does not follow standard distribution packaging structures.

### Key Characteristics of `/opt`
- **Self-Contained Subdirectories:** Applications installed in `/opt` store all their binaries, libraries, configuration, and documentation inside a single dedicated folder (e.g., `/opt/google/chrome`, `/opt/gitlab`, `/opt/slack`).
- **Easy Uninstallation:** Removing a 3rd-party application installed in `/opt` is as simple as deleting its subdirectory (`sudo rm -rf /opt/app_name`).

```text
/opt/
├── gitlab/
│   ├── bin/
│   ├── embedded/
│   └── etc/
├── google/
│   └── chrome/
└── nexus/
    └── nexus-3.37.0/
```

---

## 🌐 2. The `/srv` Directory (Service Data)

`/srv` (short for **Service Data**) contains site-specific data served directly by the system to external clients (e.g., Web server data, FTP server files, VCS repositories, Rsync shares).

### Key Characteristics of `/srv`
- **Protocol/Service Structured:** Content inside `/srv` is traditionally structured by protocol or service name:
  - `/srv/www/`: Web server static HTML/CSS files.
  - `/srv/ftp/`: FTP server download directories.
  - `/srv/git/`: Central bare Git repositories.
- **Multi-Disk Storage Flexibility:** High-capacity SAN/NAS volumes can be mounted directly to `/srv` without polluting `/var` or `/home`.

---

## ⚖️ Summary Matrix: `/opt` vs `/srv` vs `/usr/local`

| Directory | Primary Contents | Installed By | Example Path |
| :--- | :--- | :--- | :--- |
| **`/opt`** | Self-contained 3rd-party software packages. | Vendor installers / Tarballs | `/opt/gitlab/` |
| **`/srv`** | Data served outwards to web/FTP/Git clients. | SysAdmin / Application services | `/srv/www/htdocs/` |
| **`/usr/local`** | Custom binaries compiled manually from source. | SysAdmin (`make install`) | `/usr/local/bin/custom_tool` |
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - home and root](./06-home-and-root.md) | [README](./README.md) | [08 - tmp and run](./08-tmp-and-run.md) |
