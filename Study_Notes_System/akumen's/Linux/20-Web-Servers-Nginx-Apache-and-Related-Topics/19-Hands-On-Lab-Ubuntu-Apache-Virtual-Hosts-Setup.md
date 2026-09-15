# 19. Hands-On Lab: Ubuntu Apache Virtual Hosts Setup

## Lab Objective
Configure Apache 2.4 on Ubuntu with Event MPM and multi-site Virtual Hosts.

## Step-by-Step Execution

1. **Install Apache & Switch to Event MPM:**
   ```bash
   sudo apt update && sudo apt install -y apache2
   sudo a2dismod mpm_prefork
   sudo a2enmod mpm_event
   sudo systemctl restart apache2
   ```

2. **Configure Virtual Host for `site1.local`:**
   Create `/etc/apache2/sites-available/site1.conf`:
   ```apache
   <VirtualHost *:80>
       ServerName site1.local
       DocumentRoot /var/www/site1

       <Directory /var/www/site1>
           AllowOverride None
           Require all granted
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/site1_error.log
       CustomLog ${APACHE_LOG_DIR}/site1_access.log combined
   </VirtualHost>
   ```

3. **Provision Document Root & Enable Site:**
   ```bash
   sudo mkdir -p /var/www/site1
   echo "<h1>Hello from Apache Site 1</h1>" | sudo tee /var/www/site1/index.html
   sudo a2ensite site1.conf
   sudo apache2ctl configtest
   sudo systemctl reload apache2
   ```
