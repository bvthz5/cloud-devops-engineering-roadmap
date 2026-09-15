# 18. Hands-On Lab: Ubuntu Nginx Reverse Proxy & TLS Setup

## Lab Objective
Configure Nginx on Ubuntu as a TLS-terminated reverse proxy forwarding web traffic to a NodeJS application running on port 3000.

## Step-by-Step Lab Execution

1. **Install Nginx:**
   ```bash
   sudo apt update && sudo apt install -y nginx
   sudo systemctl enable --now nginx
   ```

2. **Create Nginx Server Block:**
   Create `/etc/nginx/sites-available/app.conf`:
   ```nginx
   server {
       listen 80;
       server_name app.lab.local;

       location / {
           proxy_pass http://127.0.0.1:3000;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

3. **Enable Site & Test Configuration:**
   ```bash
   sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl reload nginx
   ```

4. **Obtain SSL Certificate via Certbot:**
   ```bash
   sudo certbot --nginx -d app.lab.local
   ```

5. **Verify Live Proxy Connection:**
   ```bash
   curl -I -k https://app.lab.local
   ```
