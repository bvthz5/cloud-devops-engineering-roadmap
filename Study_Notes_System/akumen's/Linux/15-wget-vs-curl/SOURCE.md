# wget vs curl

## Purpose
Understand the differences between Linux's two primary command-line network tools, and master their syntax for downloading files, interacting with APIs, and troubleshooting connections.

```text
The Core Decision
Download/Mirror  → wget
Interact/REST    → curl
```

## Source Foundation

The supplied material covers the core philosophical comparison between `wget` (the autonomous downloader) and `curl` (the versatile API client). It details basic downloading, custom filenames (`-O` vs `-o`), handling 301/302 redirects (`-L`), and resuming broken connections (`-c` / `-C -`).

For `curl`, it covers REST API interactions (GET, POST, PUT, DELETE), manipulating headers (`-H`), handling JSON payloads (`-d`), verbose debugging (`-v`), authentication (Basic and Bearer tokens), and file uploads (`-F`, `-T`). For `wget`, it covers recursive website mirroring (`-m`, `-k`, `-p`).

The original source concepts have been expanded into a structured study module with specific DevOps CI/CD scenarios, troubleshooting checklists, and practical labs.

## Rule of thumb
*   Need a massive tarball to survive a VPN drop? → `wget -c`
*   Need an offline copy of a website? → `wget -m -k -p`
*   Need to test a REST API with JSON? → `curl -X POST -H ... -d ...`
*   Need to know why a connection is failing? → `curl -v`

## Learning path
*   `wget` vs `curl` fundamentals
*   When to use each tool
*   File downloads
*   Custom filenames
*   Redirects
*   Resuming interrupted downloads
*   `curl` GET/POST/PUT/DELETE
*   JSON API requests
*   Headers and verbose debugging
*   Authentication and Bearer tokens
*   File uploads
*   Website mirroring with `wget`
*   Common flags and scripting
*   DevOps/CI/CD scenarios
*   Troubleshooting
*   Interview Q&A
*   Hands-on terminal exercises
*   MCQs
*   Quick revision
*   Related networking/DevOps topics
