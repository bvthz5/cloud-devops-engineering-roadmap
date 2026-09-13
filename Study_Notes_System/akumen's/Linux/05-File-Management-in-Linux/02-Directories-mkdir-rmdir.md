# 02 - Directory Management: `mkdir` and `rmdir`

Managing directory hierarchies cleanly is essential for project scaffolding, build automation, and server configuration.

---

## 1. `mkdir` — Make Directories

The `mkdir` utility creates one or more new directories on the filesystem.

### Basic Usage:
```bash
# Create a single folder:
$ mkdir my_project

# Create multiple sibling folders simultaneously:
$ mkdir config logs data
```

---

## 2. Advanced `mkdir` Options

### The Essential `-p` (Parents) Flag:
By default, if you attempt to create a nested directory where the parent does not yet exist, `mkdir` fails:
```bash
$ mkdir a/b/c
mkdir: cannot create directory ‘a/b/c’: No such file or directory
```
Adding **`-p`** tells `mkdir` to automatically create any missing intermediate parent directories:
```bash
$ mkdir -p a/b/c
# (Creates 'a', then 'b' inside 'a', then 'c' inside 'b')
```
> [!TIP]
> **Idempotent Automation:** In shell scripts and CI/CD pipelines, always use `mkdir -p`. If the target directory already exists, `mkdir -p` quietly succeeds without throwing an error code!

### Setting Custom Permissions with `-m` (Mode):
Instead of running `mkdir` followed by `chmod`, set the octal permission mode directly at creation time:
```bash
# Create a private directory accessible only to the owner (0700):
$ mkdir -m 700 ~/.secure_keys
$ ls -ld ~/.secure_keys
drwx------ 2 ubuntu ubuntu 4096 Sep 13 11:00 /home/ubuntu/.secure_keys
```

### Verbose Mode with `-v`:
```bash
$ mkdir -pv app/{frontend,backend}/src
mkdir: created directory 'app'
mkdir: created directory 'app/frontend'
mkdir: created directory 'app/frontend/src'
mkdir: created directory 'app/backend'
mkdir: created directory 'app/backend/src'
```

---

## 3. `rmdir` — Remove Empty Directories

The `rmdir` command removes directories **if and only if they are completely empty**.

```bash
$ rmdir old_folder
```

### Safety by Design:
If a directory contains even a single hidden file (`.bashrc`) or subfolder, `rmdir` refuses to delete it:
```bash
$ rmdir app
rmdir: failed to remove 'app': Directory not empty
```
This strict protection prevents accidental mass data loss.

### Recursive Parent Cleanup with `-p`:
```bash
# Removes 'c', then if 'b' becomes empty removes 'b', then if 'a' becomes empty removes 'a':
$ rmdir -p a/b/c
```
*Note:* To delete a directory along with all its nested files and subfolders, use `rm -rf` (covered in the next module).
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Navigation ls cd pwd](./01-Navigation-ls-cd-pwd.md) | [README](./README.md) | [03 - Delete rm](./03-Delete-rm.md) |
