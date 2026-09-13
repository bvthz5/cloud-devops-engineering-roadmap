# 09 - Real-World DevOps Scenarios

Permissions are not just theoretical; they are the gatekeepers of security and functionality in production environments. Here are the most common scenarios DevOps engineers encounter.

---

## 🌐 Scenario 1: Web Server Document Root

**The Problem:** You deployed a web application to `/var/www/html`, but the Nginx web server is returning a `403 Forbidden` error to users.

**The Fix:**
A web server runs as a specific user (usually `www-data`, `nginx`, or `apache`). It needs to be able to read the files, and enter the directories.

1.  **Set Ownership:** Make the web server user the owner of the files.
    ```bash
    sudo chown -R www-data:www-data /var/www/html
    ```
2.  **Set Directory Permissions (755):** Directories need `x` to be entered, and `r` to be listed.
    ```bash
    sudo find /var/www/html -type d -exec chmod 755 {} \;
    ```
3.  **Set File Permissions (644):** Files should be readable, but not executable (unless they are CGI scripts).
    ```bash
    sudo find /var/www/html -type f -exec chmod 644 {} \;
    ```

---

## 🔑 Scenario 2: SSH Keys (`Permission denied (publickey)`)

**The Problem:** You are trying to SSH into a server using your private key (`~/.ssh/id_rsa`), but SSH rejects it with a warning about "UNPROTECTED PRIVATE KEY FILE!".

**The Fix:**
SSH client enforces strict security. If a private key is readable by anyone other than the owner, SSH refuses to use it, assuming it has been compromised.

1.  **Secure the Private Key (600):** Only the owner can read/write.
    ```bash
    chmod 600 ~/.ssh/id_rsa
    ```
2.  **Secure the `.ssh` Directory (700):** Only the owner can enter the directory.
    ```bash
    chmod 700 ~/.ssh
    ```
3.  **Secure Authorized Keys (644 or 600):** The public keys that allow access.
    ```bash
    chmod 644 ~/.ssh/authorized_keys
    ```

---

## 👥 Scenario 3: Creating a Collaborative Shared Directory

**The Problem:** You have a team of developers. You want them all to be able to read, write, and create files in `/shared/code`. However, if Alice creates a file, Bob cannot edit it because Alice owns it.

**The Fix:**
Use a shared group and the **SGID** (Set Group ID) special permission.

1.  **Create the Group & Add Users:**
    ```bash
    sudo groupadd devteam
    sudo usermod -aG devteam alice
    sudo usermod -aG devteam bob
    ```
2.  **Create the Directory and Set Ownership:**
    ```bash
    sudo mkdir -p /shared/code
    sudo chown root:devteam /shared/code
    ```
3.  **Set Permissions with SGID (2775):**
    ```bash
    sudo chmod 2775 /shared/code
    ```
    *   `2` (SGID) forces all new files created inside to inherit the `devteam` group.
    *   `7` (Owner: root) has full control.
    *   `7` (Group: devteam) can read, write, and enter.
    *   `5` (Others) can only read and enter.

---

## 🗑️ Scenario 4: A Secure Temporary Upload Folder

**The Problem:** An application accepts user uploads to `/var/app/uploads`. Everyone needs to write to this folder, but users should not be able to delete each other's uploaded files.

**The Fix:**
Use the **Sticky Bit**. This is exactly how `/tmp` works.

1.  **Create the Directory:**
    ```bash
    sudo mkdir -p /var/app/uploads
    ```
2.  **Set Permissions with Sticky Bit (1777):**
    ```bash
    sudo chmod 1777 /var/app/uploads
    ```
    *   `1` (Sticky Bit) prevents deletion of files by non-owners.
    *   `777` grants read, write, and execute to everyone.
