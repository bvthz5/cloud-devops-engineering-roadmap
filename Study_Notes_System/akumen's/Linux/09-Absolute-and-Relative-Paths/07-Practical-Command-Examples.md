# 07 - Practical Command Examples for Path Resolution

Real-world terminal command examples demonstrating path navigation, file manipulation, and wildcard matching across Linux commands.

---

## 🛠️ Command Navigation Matrix

```bash
# 1. Change to absolute directory path
cd /var/log/nginx

# 2. Change to relative subdirectory
cd html

# 3. Change to parent directory
cd ..

# 4. Change directly to current user's home directory
cd ~

# 5. Return to previous working directory ($OLDPWD)
cd -

# 6. Copy file using absolute source and relative destination (.)
cp /etc/fstab .

# 7. Move file using relative source and absolute destination
mv config.backup /var/backups/

# 8. Create symbolic link using absolute paths (Best practice for symlinks)
ln -s /var/log/nginx/access.log /home/ubuntu/access_symlink.log
```

---

## 💻 Wildcard Code Recipes

```bash
# Copy all .conf files from /etc/nginx to local backup folder
cp /etc/nginx/*.conf ./backup/

# Find all files matching access_2026-09-*.log
ls -lh /var/log/nginx/access_2026-09-??.log

# Compress all tarballs matching backup_v[1-5].tar.gz
tar -czvf combined_backup.tar.gz backup_v[1-5].tar.gz
```

---

## ⬅️ Navigation
- Previous: [06 - Wildcards & Shell Globbing](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/06-Wildcards-and-Shell-Globbing.md)
- Next: [08 - Real-World Production Scenarios](file:///c:/Users/binil/OneDrive/Desktop/cloud-devops-engineering-roadmap/Study_Notes_System/akumen's/Linux/09-Absolute-and-Relative-Paths/08-Real-World-Production-Scenarios.md)
