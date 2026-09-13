# 12 - Hands-On Terminal Practice

This lab provides a safe environment to practice manipulating permissions and ownership. 

> **Prerequisites:** You need access to a Linux terminal. Some commands require `sudo` privileges.

---

## 🛠️ Lab Setup

Create a playground directory and some files to work with.

```bash
mkdir ~/permissions-lab
cd ~/permissions-lab

touch script.sh data.txt secret.key
mkdir shared_folder

ls -l
```
*Note the default permissions assigned to the new files and directories.*

---

## ⬛ Level 1: Symbolic `chmod`

**Tasks:**
1. Make `script.sh` executable for the User (Owner).
2. Remove read access from Others for `data.txt`.
3. Strip all permissions from Group and Others for `secret.key`.

**Commands:**
```bash
chmod u+x script.sh
chmod o-r data.txt
chmod go= secret.key
```

**Verify with `ls -l`:**
*   `script.sh` should have an `x` in the first triad.
*   `data.txt` should end in `---`.
*   `secret.key` should look like `-rw-------`.

---

## 🟫 Level 2: Octal `chmod`

**Tasks:**
1. Set `data.txt` to standard file permissions (Owner rw, Group/Others r).
2. Set `script.sh` to standard executable permissions (Owner rwx, Group/Others rx).
3. Set `shared_folder` wide open (rwx for everyone).

**Commands:**
```bash
chmod 644 data.txt
chmod 755 script.sh
chmod 777 shared_folder
```

**Verify with `ls -l`:**
*   `data.txt` -> `-rw-r--r--`
*   `script.sh` -> `-rwxr-xr-x`
*   `shared_folder` -> `drwxrwxrwx`

---

## 🟦 Level 3: The Sticky Bit & Directory Execute

**Tasks:**
1. You made `shared_folder` `777`. Now apply the Sticky Bit so users can't delete each other's files.
2. Create a folder `secret_dir`. Remove execute from it.
3. Put a file inside it and try to read it.

**Commands:**
```bash
# 1. Sticky Bit
chmod +t shared_folder
# Verify: ls -ld shared_folder shows 't' at the end.

# 2. Directory Execute experiment
mkdir secret_dir
touch secret_dir/visible.txt
chmod a-x secret_dir

# 3. Try to access
ls -l secret_dir          # Will likely show errors accessing metadata
cat secret_dir/visible.txt # Will result in "Permission denied"
```

**Cleanup:** Restore execute so you can delete it later.
```bash
chmod a+x secret_dir
```

---

## 🟥 Level 4: Ownership (`chown` / `chgrp`)

*(Requires `sudo`)*

**Tasks:**
1. Change the User Owner of `data.txt` to `root`.
2. Try to edit `data.txt` with your normal user (using `vi` or `echo`). It should fail.
3. Change the owner back to yourself, and the group to `root` simultaneously.

**Commands:**
```bash
# 1. Change Owner
sudo chown root data.txt
ls -l data.txt

# 2. Try to edit
echo "hello" > data.txt   # Permission denied!

# 3. Change Owner and Group
sudo chown $(whoami):root data.txt
ls -l data.txt
```

---

## 🏆 Challenge: The Web Server Scenario

1. Create a directory structure simulating a web server: `/tmp/www/html`.
2. Inside `html`, create `index.html`.
3. Use a single command to recursively change the ownership of `/tmp/www` to `root:www-data` (assuming `www-data` group exists; if not, use `root:daemon`).
4. Use `find` to set all directories inside `/tmp/www` to `755`.
5. Use `find` to set all files inside `/tmp/www` to `644`.

**Solution:**
```bash
mkdir -p /tmp/www/html
touch /tmp/www/html/index.html

sudo chown -R root:www-data /tmp/www/

sudo find /tmp/www -type d -exec chmod 755 {} \;
sudo find /tmp/www -type f -exec chmod 644 {} \;

ls -la /tmp/www/html
```
