# 03 - File Downloads and Filenames

The most common use case for both `wget` and `curl` is simply grabbing a file from the internet.

---

## 📥 Basic Download

### Using `wget`
By default, `wget` downloads the URL and saves it in the current directory, keeping the filename from the URL.

```bash
wget https://example.com/software.tar.gz
# Result: A file named 'software.tar.gz' is saved in your current directory.
```

### Using `curl`
By default, `curl` prints the contents of the URL to the terminal screen (`stdout`). If you run `curl https://example.com/software.tar.gz`, your terminal will be flooded with unreadable binary garbage.

To force `curl` to behave like `wget` and save the file using the URL's filename, you must use the `-O` (capital O) flag.

```bash
curl -O https://example.com/software.tar.gz
# Result: A file named 'software.tar.gz' is saved.
```

---

## ✏️ Custom Filenames

Sometimes the URL doesn't have a friendly filename (e.g., `https://api.example.com/v1/download?id=12345`). You want to specify the output name manually.

### Using `wget` (Capital `-O`)
Use the **capital `-O`** flag to specify the output document.

```bash
wget -O my_custom_name.zip https://api.example.com/v1/download?id=12345
```

### Using `curl` (Lowercase `-o`)
Use the **lowercase `-o`** flag to specify the output file.

```bash
curl -o my_custom_name.zip https://api.example.com/v1/download?id=12345
```

> **The `O` vs `o` Trap:** This is a classic point of confusion. 
> *   `wget`: `-O` (Capital) is custom name.
> *   `curl`: `-O` (Capital) is keep original name. `-o` (Lowercase) is custom name.

---

## 🤫 Silent Mode (Suppressing Progress Bars)

When writing scripts, you usually don't want the progress bar cluttering your logs.

### Using `wget` (`-q`)
```bash
wget -q https://example.com/file.txt
```

### Using `curl` (`-s`)
```bash
curl -s -O https://example.com/file.txt
```
*(Note: If `curl` fails in silent mode, it outputs nothing. It is often paired with `-S` to show errors even when silent: `curl -sS`).*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - When to use each](./02-When-to-use-each.md) | [README](./README.md) | [04 - Redirects and Resumes](./04-Redirects-and-Resumes.md) |
