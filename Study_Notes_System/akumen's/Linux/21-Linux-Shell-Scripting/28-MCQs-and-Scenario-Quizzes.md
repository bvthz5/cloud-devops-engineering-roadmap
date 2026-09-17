# 28 — MCQs and Scenario Quizzes

## Question 1
What is the return value of `echo $?` immediately after a successful command?

- A) 1
- B) 0
- C) -1
- D) true

**Answer**: **B**
*Explanation*: In Linux, exit status `0` indicates success, while non-zero values (1-255) indicate errors.

---

## Question 2
Which option prevents `read` from interpreting backslashes as escape characters?

- A) `-s`
- B) `-p`
- C) `-r`
- D) `-a`

**Answer**: **C**
*Explanation*: The `-r` flag forces raw input mode, treating backslashes literally.

---

## Question 3
How do you declare an associative array (key-value map) in Bash?

- A) `array=(key value)`
- B) `declare -a array`
- C) `declare -A array`
- D) `map array`

**Answer**: **C**
*Explanation*: `declare -A` initializes an associative array, while `declare -a` initializes an indexed array.

---

## Question 4
What does `set -e` do?

- A) Enables echo of all executed commands.
- B) Exits script immediately if a command returns a non-zero exit status.
- C) Treats unset variables as errors.
- D) Enables strict pipe error reporting.

**Answer**: **B**
*Explanation*: `set -e` (errexit) terminates script execution immediately upon command failure.

---

## Question 5
Which utility generates a unique temporary file path safely?

- A) `touch`
- B) `mktemp`
- C) `tempfile`
- D) `tmp`

**Answer**: **B**
*Explanation*: `mktemp` creates random unique temporary filenames with safe 0600 permissions.
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [27 - Comprehensive Interview Q&A](./27-Comprehensive-Interview-Questions-and-Answers.md) | [README](./README.md) | [29 - Quick Revision Notes](./29-Quick-Revision-Notes.md) |
