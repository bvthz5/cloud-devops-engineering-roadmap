# 15 - Hands-On Terminal Practice

This lab provides a safe environment to practice manipulating processes, sending signals, and managing jobs. 

> **Prerequisites:** A Linux terminal. Some commands require `sudo`.

---

## 🛠️ Level 1: Viewing and Identifying

**Tasks:**
1. Find your current shell's PID.
2. View the full process tree of your system.
3. Start a process that sleeps for 500 seconds in the background, and find its PID using `pgrep`.

**Commands:**
```bash
# 1. Find your shell's PID (echo the special variable $$)
echo $$

# 2. View the process tree
pstree -p

# 3. Start a background sleeper and find it
sleep 500 &
pgrep sleep
```

---

## ⬛ Level 2: Job Control

**Tasks:**
1. Bring that `sleep 500` job back to the foreground.
2. Suspend it (pause it).
3. Verify it is stopped using `jobs`.
4. Resume it in the background.

**Commands:**
```bash
# 1. Bring to foreground
fg %1

# 2. Suspend it
# Press: Ctrl+Z

# 3. Verify it is stopped
jobs
# Output should show: [1]+  Stopped  sleep 500

# 4. Resume in background
bg %1
jobs
# Output should show: [1]+  Running  sleep 500 &
```

---

## 🟫 Level 3: Signals and Killing

**Tasks:**
1. Attempt to kill the background sleep process politely.
2. Verify it is dead.
3. Start three new sleep processes in the background.
4. Kill all three simultaneously using a single command.

**Commands:**
```bash
# 1. Polite kill (assuming Job ID 1, or use the PID)
kill %1
# Alternatively, if PID was 1234: kill 1234

# 2. Verify
jobs
# Should show "Terminated"

# 3. Start three processes
sleep 600 &
sleep 601 &
sleep 602 &

# 4. Mass kill by name
pkill sleep
# Verify with: jobs
```

---

## 🟦 Level 4: Niceness and Priority

*(May require `sudo` depending on your environment)*

**Tasks:**
1. Create a CPU-intensive process (a simple bash loop) in the background.
2. Check its niceness value using `top` or `ps`.
3. Lower its priority (increase niceness) to +15.
4. Kill the loop.

**Commands:**
```bash
# 1. Start an infinite loop in the background
while true; do true; done &

# 2. Find its PID (it will be eating 100% of one core)
top 
# Note the PID, let's say 4455. Note the 'NI' column is 0.
# Press 'q' to exit top.

# 3. Renice it
renice -n 15 -p 4455

# 4. Verify in top again (NI should be 15). Then kill it.
kill -9 4455
```

---

## 🟥 Level 5: Exploring `/proc`

**Tasks:**
1. Start a process with a very specific, long name/argument.
2. Find its PID.
3. Dive into its `/proc` directory and read its `cmdline`.

**Commands:**
```bash
# 1. Start a uniquely named process
sleep 987654321 &

# 2. Find PID
pgrep -f 987654321
# Let's say it returns 7788

# 3. Explore /proc
cat /proc/7788/cmdline
# Because it's null-separated, format it:
cat /proc/7788/cmdline | tr '\0' ' '

# Cleanup
kill 7788
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Interview QA](./14-Interview-QA.md) | [README](./README.md) | [16 - MCQs](./16-MCQs.md) |
