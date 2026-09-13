# 11 - MCQs

15 multiple-choice questions. Attempt each before revealing the answer.

---

**Q1. Which tool is primarily used to search text for matching lines?**

- A. awk
- B. sed
- C. grep
- D. sort

<details>
<summary>Answer</summary>

**C — grep**

grep (Global Regular Expression Print) is specifically designed to search and filter lines by pattern.

</details>

---

**Q2. Which tool is primarily used for stream editing (substitution, deletion)?**

- A. grep
- B. sed
- C. awk
- D. head

<details>
<summary>Answer</summary>

**B — sed**

sed (Stream Editor) applies editing commands — most commonly `s/old/new/g` substitution — to each line of input.

</details>

---

**Q3. Which tool is best suited for column extraction and numeric calculations?**

- A. awk
- B. grep
- C. cat
- D. less

<details>
<summary>Answer</summary>

**A — awk**

awk splits records into fields and supports arithmetic operations, making it ideal for structured data processing.

</details>

---

**Q4. Which grep option performs a case-insensitive search?**

- A. -c
- B. -i
- C. -v
- D. -n

<details>
<summary>Answer</summary>

**B — -i**

`grep -i "pattern" file` matches regardless of case (Alice, alice, ALICE all match "alice").

</details>

---

**Q5. Which grep option displays line numbers alongside matches?**

- A. -n
- B. -l
- C. -r
- D. -o

<details>
<summary>Answer</summary>

**A — -n**

`grep -n "pattern" file` prefixes each matching line with its line number.

</details>

---

**Q6. Which grep option shows lines that do NOT match the pattern?**

- A. -v
- B. -i
- C. -E
- D. -w

<details>
<summary>Answer</summary>

**A — -v**

`grep -v "pattern" file` inverts the match — outputs lines where the pattern is absent.

</details>

---

**Q7. What does the `s` in `sed 's/old/new/g'` stand for?**

- A. Search only
- B. Substitute
- C. Sort
- D. Save

<details>
<summary>Answer</summary>

**B — Substitute**

The `s` command in sed performs text substitution: `s/pattern/replacement/flags`.

</details>

---

**Q8. Which sed option edits a file in-place (directly modifying the original file)?**

- A. -p
- B. -i
- C. -n
- D. -e

<details>
<summary>Answer</summary>

**B — -i**

`sed -i 's/old/new/g' file` modifies the file directly. Use `sed -i.bak` to keep a backup.

</details>

---

**Q9. In awk, what does `$0` represent?**

- A. First field
- B. Last field
- C. The entire current line
- D. Line number

<details>
<summary>Answer</summary>

**C — The entire current line**

`$0` always refers to the complete, unsplit current record (line).

</details>

---

**Q10. In awk, what does `$NF` represent?**

- A. First field
- B. Last field
- C. Number of lines
- D. Filename

<details>
<summary>Answer</summary>

**B — Last field**

`NF` = Number of Fields in the current record. `$NF` therefore refers to the value of the last field, regardless of how many fields exist.

</details>

---

**Q11. What does `NR` represent in awk?**

- A. Number of Records processed / current record number
- B. Number of fields in the current record
- C. Regex mode flag
- D. New row indicator

<details>
<summary>Answer</summary>

**A — Number of Records processed / current record number**

`NR` increments by 1 for each line processed. `NR > 1` is the standard way to skip a header row.

</details>

---

**Q12. What does `NF` represent in awk?**

- A. Number of files
- B. Number of fields in the current record
- C. Filename variable
- D. New field separator

<details>
<summary>Answer</summary>

**B — Number of fields in the current record**

`NF` changes with each line if line lengths vary. Use `$NF` to access the last field value.

</details>

---

**Q13. Which awk option sets the input field separator?**

- A. -F
- B. -S
- C. -f (lowercase only)
- D. -d

<details>
<summary>Answer</summary>

**A — -F**

`awk -F',' '{print $2}' file` sets the field separator to comma for CSV processing.

</details>

---

**Q14. What does the `BEGIN` block do in awk?**

- A. Runs after all input has been read
- B. Runs before any input records are processed
- C. Deletes input records
- D. Sorts the input

<details>
<summary>Answer</summary>

**B — Runs before any input records are processed**

`BEGIN { }` executes once before the first line of input is read. Commonly used to print report headers or initialize variables.

</details>

---

**Q15. What does the pipe operator `|` do?**

- A. Deletes the output of a command
- B. Sends the stdout of one command to the stdin of the next command
- C. Starts a new shell session
- D. Changes file permissions

<details>
<summary>Answer</summary>

**B — Sends stdout of one command to stdin of the next**

Pipes connect commands into data pipelines: `grep "ERROR" app.log | awk '{print $1}'`

</details>
