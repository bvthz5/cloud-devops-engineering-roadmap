# 08 - File Uploads

While `wget` is strictly a downloader, `curl` excels at uploading files to servers via various methods (HTTP POST, PUT, FTP, etc.).

---

## 📤 1. Uploading via Web Forms (Multipart POST)

If a website has a standard "File Upload" HTML form, it submits the data using the `multipart/form-data` encoding.

You can replicate this in `curl` using the **`-F`** (Form) flag. 

You specify the name of the HTML input field, followed by an `@` symbol, followed by the path to the file on your local disk.

```bash
# Assuming the HTML form had an input like: <input type="file" name="profile_pic">
curl -F "profile_pic=@/home/alice/photo.jpg" https://api.example.com/upload
```

You can combine file uploads with standard form fields:
```bash
curl -F "profile_pic=@/home/alice/photo.jpg" \
     -F "username=alice" \
     https://api.example.com/upload
```

---

## 🚀 2. Uploading via HTTP PUT or FTP (`-T`)

If you are interacting with an API that expects the raw file content in the body of a PUT request (common in object storage like AWS S3 or Artifactory), or if you are uploading via FTP/SFTP, use the **`-T`** (Transfer) flag.

### HTTP PUT Upload
```bash
curl -T my_backup.tar.gz https://api.example.com/storage/backups/
```
*(By default, `-T` changes the HTTP method to PUT).*

### FTP Upload
```bash
curl -T my_backup.tar.gz -u username:password ftp://ftp.example.com/backups/
```

### SFTP Upload (Secure)
```bash
curl -T my_backup.tar.gz -u username:password sftp://sftp.example.com/~/backups/
```
*(Note: For SFTP, you often use SSH key authentication rather than passwords. While `curl` can do this, standard `scp` or `sftp` command-line tools are usually preferred for ease of use).*

---

## 🚦 Handling Large Uploads

If you are uploading massive files over slow connections, `curl` will keep the connection alive. However, if the connection drops, `curl` does support resuming uploads with the same `-C -` flag used for resuming downloads.

```bash
# Resume an interrupted upload
curl -C - -T massive_db_dump.sql sftp://server.example.com/data/
```
