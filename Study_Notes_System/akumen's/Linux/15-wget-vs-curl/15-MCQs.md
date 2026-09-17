# 15 - MCQs

15 multiple-choice questions to test your understanding. Attempt each before revealing the answer.

---

**Q1. By default, what does `wget` do with the data it downloads?**

- A. Prints it to the terminal screen (stdout).
- B. Saves it to a file using the URL's filename.
- C. Saves it to a temporary `/tmp` directory.
- D. Prompts the user for a filename.

<details>
<summary>Answer</summary>

**B — Saves it to a file using the URL's filename.**

This is `wget`'s default behavior, distinguishing it from `curl`.

</details>

---

**Q2. By default, what does `curl` do with the data it downloads?**

- A. Prints it to the terminal screen (stdout).
- B. Saves it to a file using the URL's filename.
- C. Saves it to a temporary `/tmp` directory.
- D. Prompts the user for a filename.

<details>
<summary>Answer</summary>

**A — Prints it to the terminal screen (stdout).**

This makes `curl` ideal for piping into other tools like `jq` or `bash`.

</details>

---

**Q3. Which flag forces `curl` to save the output to a file using the remote server's filename?**

- A. `-o` (lowercase)
- B. `-O` (uppercase)
- C. `-s`
- D. `-w`

<details>
<summary>Answer</summary>

**B — `-O` (uppercase)**

Remember: `curl -O` keeps the original name. `curl -o` allows you to specify a custom name.

</details>

---

**Q4. You run `curl http://github.com` but receive an empty response or a "301 Moved Permanently" message. Why?**

- A. GitHub is down.
- B. `curl` requires `sudo` privileges.
- C. `curl` does not follow HTTP redirects by default.
- D. You forgot the `-O` flag.

<details>
<summary>Answer</summary>

**C — `curl` does not follow HTTP redirects by default.**

You must add the `-L` (Location) flag to force `curl` to follow redirects.

</details>

---

**Q5. How do you tell `wget` to automatically resume a partially downloaded file?**

- A. `wget --resume <URL>`
- B. `wget -r <URL>`
- C. `wget -C <URL>`
- D. `wget -c <URL>`

<details>
<summary>Answer</summary>

**D — `wget -c <URL>`**

The `-c` (continue) flag sends a Range request to the server to pick up where it left off.

</details>

---

**Q6. You are writing a script and want `curl` to operate without printing the progress bar, but you STILL want to see error messages if it fails. Which flags do you use?**

- A. `-q`
- B. `-s`
- C. `-sS`
- D. `-v`

<details>
<summary>Answer</summary>

**C — `-sS`**

`-s` enables silent mode, but `-S` (Show errors) overrides silent mode specifically for error output.

</details>

---

**Q7. Which tool is capable of recursively crawling a website and downloading all linked files?**

- A. `wget` only
- B. `curl` only
- C. Both `wget` and `curl`
- D. Neither

<details>
<summary>Answer</summary>

**A — `wget` only**

`curl` is not a web crawler; it fetches exactly what you point it at. `wget`'s `-r` flag enables recursion.

</details>

---

**Q8. How do you pass a custom HTTP header (like a Bearer Token) in `curl`?**

- A. `curl -t "Token" <URL>`
- B. `curl -H "Authorization: Bearer <token>" <URL>`
- C. `curl -X "Authorization: Bearer <token>" <URL>`
- D. `curl --header=<token> <URL>`

<details>
<summary>Answer</summary>

**B — `curl -H "Authorization: Bearer <token>" <URL>`**

The `-H` flag is used for injecting any custom HTTP header.

</details>

---

**Q9. If you want to send a JSON payload in a POST request using `curl`, which flag do you use for the data?**

- A. `-X`
- B. `-F`
- C. `-d`
- D. `-T`

<details>
<summary>Answer</summary>

**C — `-d`**

`-d` (or `--data`) sends the payload in the HTTP body. (Remember to also set the `-H "Content-Type: application/json"` header).

</details>

---

**Q10. What does the `-I` (uppercase i) flag do in `curl`?**

- A. Insecure (ignores SSL).
- B. Includes the response headers in the output before the body.
- C. Fetches ONLY the HTTP headers (sends a HEAD request).
- D. Interactive mode.

<details>
<summary>Answer</summary>

**C — Fetches ONLY the HTTP headers (sends a HEAD request).**

This is useful for checking content length, server type, or cookies without downloading the actual file.

</details>

---

**Q11. You need to test an API endpoint but the server has a self-signed SSL certificate. `curl` throws an error. How do you bypass this?**

- A. `curl --ignore-ssl <URL>`
- B. `curl -k <URL>`
- C. `curl -v <URL>`
- D. You must use `wget` instead.

<details>
<summary>Answer</summary>

**B — `curl -k <URL>`**

`-k` (or `--insecure`) tells `curl` to skip SSL certificate validation.

</details>

---

**Q12. What does the `-v` flag do in `curl`?**

- A. Validates the JSON response.
- B. Displays the curl Version.
- C. Enables Verbose mode, showing the full request/response headers and connection details.
- D. Views the file without saving it.

<details>
<summary>Answer</summary>

**C — Enables Verbose mode.**

It is the primary troubleshooting tool when an API request is failing, allowing you to see exactly what is being sent and received.

</details>

---

**Q13. You want to upload a file to a server via FTP using `curl`. Which flag specifies the file to upload?**

- A. `-d`
- B. `-X`
- C. `-T`
- D. `-u`

<details>
<summary>Answer</summary>

**C — `-T`**

`-T` (Transfer) is used to upload a raw file, usually via FTP, SFTP, or HTTP PUT.

</details>

---

**Q14. In `wget`, what does the `-m` flag do?**

- A. Mutes all output.
- B. Mirrors a website recursively, checking timestamps to only download updated files.
- C. Modifies the user-agent.
- D. Moves the file to a new directory.

<details>
<summary>Answer</summary>

**B — Mirrors a website recursively, checking timestamps to only download updated files.**

It is equivalent to running `-r -N -l inf --no-remove-listing`.

</details>

---

**Q15. In bash scripts, what is the purpose of piping `curl` output to `jq`?**

- A. To encrypt the download.
- B. To format, colorize, and parse JSON responses from REST APIs.
- C. To follow HTTP redirects automatically.
- D. To save the output to a file silently.

<details>
<summary>Answer</summary>

**B — To format, colorize, and parse JSON responses from REST APIs.**

`jq` is the standard command-line JSON processor used extensively with `curl`.

</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [14 - Hands On Terminal Practice](./14-Hands-On-Terminal-Practice.md) | [README](./README.md) | [16 - Quick Revision](./16-Quick-Revision.md) |
