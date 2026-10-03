1. buat file `/etc/nginx/sites-available/your-domain.com`, nama domain di sesuaikan dengan domain anda sendiri ya
   ```bash
   server {
    listen 80;
    listen [::]:80;

    server_name stag-erp.auranex.tech;

    access_log /var/log/nginx/stag-erp.access.log;
    error_log  /var/log/nginx/stag-erp.error.log;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
    add_header X-XSS-Protection "1; mode=block";

    client_max_body_size 100M;

    location / {

        proxy_pass         http://127.0.0.1:8001;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection 'upgrade';
        proxy_set_header   Host              $host;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;

        proxy_connect_timeout 60s;
        proxy_send_timeout    60s;
        proxy_read_timeout    300s;

    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    location ~ /\.(?!well-known).* {

        deny all;

    }

}
   ```
2. Lalu jalankan langkah berikut:
```bash
# 1. Aktifkan config
sudo ln -s /etc/nginx/sites-available/stag-erp.auranex.tech /etc/nginx/sites-enabled/

# 2. Test config
sudo nginx -t

# 3. Reload Nginx
sudo systemctl reload nginx

# 4. Install SSL dengan Certbot
sudo certbot --nginx -d stag-erp.auranex.tech
```

3. aktivasi SSL untuk domain baru atau tenant baru
   ```
   sudo certbot --nginx -d baru.domain-tenant.com
   ```