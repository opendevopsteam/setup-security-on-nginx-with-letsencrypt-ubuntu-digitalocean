# setup-security-on-nginx-with-letsencrypt-ubuntu-digitalocean
🌊 How To Secure Nginx with Let's Encrypt on Ubuntu 18.04 in DigitalOcean

**Step 1**

*Installing Certbot*

```bash
sudo add-apt-repository ppa:certbot/certbot
```

```bash
sudo apt install python-certbot-nginx
```

**Step 2**

*Confirming Nginx’s Configuration*

```bash
sudo nano /etc/nginx/sites-available/example.com
```

**/etc/nginx/sites-available/example.com**

```bash
...
server_name example.com www.example.com;
...
```

```bash
sudo nginx -t
```

```bash
sudo systemctl reload nginx
```

**Step 3**

*Allowing HTTPS Through the Firewall*

```bash
sudo ufw status
```

```bash
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

```bash
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

```bash
sudo ufw status
```

```bash
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx Full                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx Full (v6)            ALLOW       Anywhere (v6)
```

**Step 4**

*Obtaining an SSL Certificate*

```bash
sudo certbot --nginx -d example.com -d www.example.com
```

```bash
Output
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
-------------------------------------------------------------------------------
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

```bash
Output
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on 2018-07-23. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

**Step 5**

*Verifying Certbot Auto-Renewal*

```bash
sudo certbot renew --dry-run
```
