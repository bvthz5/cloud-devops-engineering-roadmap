# 09 - Website Mirroring (wget)

One of `wget`'s absolute superpowers is its ability to crawl a website, follow hyperlinks, and download an entire directory structure or a complete static copy of a website. `curl` cannot do this.

---

## 🕷️ Recursive Downloading (`-r`)

If you want to download a file and everything in the directories below it, use the `-r` (recursive) flag.

```bash
wget -r https://example.com/docs/
```
*Note: By default, `wget -r` only goes 5 levels deep to prevent accidentally downloading the entire internet. You can change this with `-l` (level): `wget -r -l 2 ...`*

---

## 🪞 The Ultimate Mirror Command (`-m`)

If you want to create a complete, offline, locally viewable copy of a website, you use the **`-m`** (mirror) flag. 

`-m` is actually a shortcut that turns on several flags simultaneously:
*   `-r` (Recursive).
*   `-N` (Timestamping: only downloads files that are newer than the local copy, great for keeping a mirror updated).
*   `-l inf` (Infinite recursion depth).

### Making it Offline-Friendly (`-k` and `-p`)
If you just use `-m`, the downloaded HTML files will still contain absolute links pointing back to `https://example.com/page2`. If you click a link locally, you will be sent back to the live internet.

To make the mirror truly offline:
*   **`-k` (Convert Links):** After the download finishes, `wget` rewrites all the links in the HTML documents so they point to the local downloaded files.
*   **`-p` (Page Requisites):** Ensures that all images, CSS, and JavaScript necessary to display the HTML page properly are downloaded, even if they live on a CDN or different domain.

### The Complete Command

```bash
wget -m -k -p https://example.com
```

---

## 🚫 Being Polite (and Avoiding Bans)

Web servers do not like being aggressively crawled. If `wget` hits a server 100 times a second, the server's firewall will likely ban your IP address.

You must throttle `wget`.

*   **`--wait=X`**: Wait X seconds between every retrieval.
*   **`--random-wait`**: Randomizes the wait time (between 0.5 and 1.5 times the `--wait` value) to make the crawler look more like a human and less like a bot.
*   **`--limit-rate=X`**: Limits the download bandwidth (e.g., `200k`).

### The Polite Mirror Command

```bash
wget -m -k -p --wait=2 --random-wait https://example.com
```

---

## 🎯 Filtering Downloads

Sometimes you only want specific file types from a directory.

*   **`-A` (Accept list):** Only download these extensions.
*   **`-R` (Reject list):** Do not download these extensions.

```bash
# Crawl the site, but ONLY download PDF files
wget -r -A.pdf https://example.com/reports/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - File Uploads](./08-File-Uploads.md) | [README](./README.md) | [10 - Common Flags Cheat Sheet](./10-Common-Flags-Cheat-Sheet.md) |
