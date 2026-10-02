# 20 - Hands-On Practice & Terminal Labs

Execute these step-by-step terminal labs to build real muscle memory in file manipulation, text streams, and Vim editing.

---

## 🧪 Lab 1: Advanced Tree Scaffolding & Archiving

```bash
# 1. Create a multi-tier project directory structure in one command:
mkdir -p ~/file_lab/project/{src,bin,docs,config}

# 2. Populate config files using brace expansion:
touch ~/file_lab/project/config/{app,db,cache}.env

# 3. Restrict permissions on the config folder (Owner only):
chmod 700 ~/file_lab/project/config
chmod 600 ~/file_lab/project/config/*.env

# 4. Create a gzipped tarball archive:
cd ~/file_lab
tar -czvf project_backup.tar.gz project/

# 5. Verify archive contents without extracting:
tar -tzvf project_backup.tar.gz
```

---

## 🧪 Lab 2: I/O Redirection & Stream Separation

```bash
# 1. Create a script that outputs to both stdout and stderr:
cat << 'EOF' > ~/file_lab/stream_test.sh
#!/bin/bash
echo "NORMAL OUTPUT: Deployment started"
echo "ERROR OUTPUT: Database connection delayed" >&2
echo "NORMAL OUTPUT: Deployment finished"
EOF

chmod +x ~/file_lab/stream_test.sh

# 2. Run script separating stdout and stderr into different log files:
~/file_lab/stream_test.sh > ~/file_lab/output.log 2> ~/file_lab/error.log

# 3. Inspect both log files:
cat ~/file_lab/output.log
cat ~/file_lab/error.log

# 4. Combine both streams into a single timestamped log using tee:
~/file_lab/stream_test.sh 2>&1 | tee -a ~/file_lab/combined.log
```

---

## 🧪 Lab 3: Inode & Link Mechanics Experiment

```bash
# 1. Create a test file:
cd ~/file_lab
echo "Original Data Content" > data.txt

# 2. Create a Hard Link and a Soft Symlink:
ln data.txt hard.link
ln -s data.txt soft.link

# 3. Inspect inode numbers and link counts:
ls -li data.txt hard.link soft.link

# OBSERVE:
# - 'data.txt' and 'hard.link' share the exact same Inode number!
# - 'soft.link' has a different Inode number and type 'l'.

# 4. Remove original file:
rm data.txt

# 5. Test reading both links:
cat hard.link   # SUCCESS: Data still accessible!
cat soft.link   # FAILS: "No such file or directory" (Broken link!)
```

---

## 🧪 Lab 4: Vim Editing Workout

```bash
# 1. Create and open a config file in Vim:
vim ~/file_lab/server.conf

# 2. Press 'i' to enter Insert Mode. Type the following lines:
# port=8080
# environment=development
# debug=true

# 3. Press 'Esc' to return to Normal Mode.
# 4. Move to line 1 and press 'yy' to yank (copy) line 1.
# 5. Move to line 3 and press 'p' to paste the copied line below.
# 6. Type ':%s/development/production/g' and press Enter to search & replace.
# 7. Type ':wq' and press Enter to save and exit!
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [19 - Interview QA](./19-Interview-QA.md) | [README](./README.md) | [21 - MCQ](./21-MCQ.md) |
