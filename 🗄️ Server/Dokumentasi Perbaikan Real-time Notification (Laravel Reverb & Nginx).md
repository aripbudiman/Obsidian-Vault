Dokumentasi ini merangkum perbaikan masalah notifikasi real-time yang tidak berjalan atau bersifat intermiten pada environment Staging (`auranex-staging-app`).

---

## 1. Perbaikan Notifikasi Real-time Tidak Berjalan (Driver Connection)

### Masalah:
Notifikasi baru hanya muncul ketika dropdown di-klik atau halaman di-refresh. 

### Penyebab:
Di file `.env` staging, konfigurasi `BROADCAST_CONNECTION` diset ke `log`. Hal ini menyebabkan semua event real-time (seperti `.notification.received`) hanya ditulis ke file log (`storage/logs/laravel.log`) dan tidak dikirimkan ke server WebSocket (Laravel Reverb).

### Solusi:
Mengubah `BROADCAST_CONNECTION` menjadi `reverb` di file `/var/www/stagging/auranex-erp/.env`:
```ini
# Sebelum
BROADCAST_CONNECTION=log

# Sesudah
BROADCAST_CONNECTION=reverb
```
*Setelah perubahan ini, cache config dibersihkan menggunakan `php artisan config:clear`.*

---

## 2. Perbaikan Koneksi Real-time Intermiten (Nginx Timeout)

### Masalah:
Notifikasi real-time kadang-kadang masuk secara instan, namun terkadang tidak masuk sama sekali hingga halaman dimuat ulang.

### Penyebab:
Konfigurasi reverse proxy Nginx untuk WebSocket di sites-available (`ws-stg.auranex.tech`, `ws-dev.auranex.tech`, dan `ws.auranex.tech`) memiliki nilai `proxy_read_timeout` yang terlalu rendah, yaitu `60s`. 

Karena interval ping default Laravel Reverb adalah tepat 60 detik, sedikit saja latensi jaringan akan memicu Nginx untuk menutup koneksi WebSocket sebelum ping berikutnya diterima. Akibatnya, browser terus menerus mengalami siklus putus-nyambung (*disconnect & reconnect*), dan notifikasi yang dikirimkan saat status koneksi terputus tidak akan diterima oleh browser.

### Solusi:
Meningkatkan `proxy_read_timeout` menjadi `3600s` (1 jam) agar koneksi WebSocket tetap terjaga secara stabil.

Modifikasi dilakukan pada file:
- `/etc/nginx/sites-available/ws-stg.auranex.tech`
- `/etc/nginx/sites-available/ws-dev.auranex.tech`
- `/etc/nginx/sites-available/ws.auranex.tech`

```nginx
location / {
    proxy_pass         http://127.0.0.1:39002; # Port disesuaikan per environment
    proxy_http_version 1.1;

    proxy_set_header   Host              $host;
    proxy_set_header   X-Real-IP         $remote_addr;
    proxy_set_header   X-Forwarded-For   $proxy_add_x_forwarded_for;
    proxy_set_header   X-Forwarded-Proto $scheme;
    proxy_set_header   Upgrade           $http_upgrade;
    proxy_set_header   Connection        "upgrade";

    proxy_connect_timeout 60s;
    proxy_send_timeout    60s;
    # Sebelum: proxy_read_timeout 60s;
    proxy_read_timeout    3600s; # Sesudah: Diubah ke 3600 detik (1 jam)

    proxy_buffering    off;
    proxy_cache_bypass $http_upgrade;
}
```
*Setelah perubahan, konfigurasi Nginx di-reload menggunakan perintah `nginx -t && systemctl reload nginx`.*

---

## 3. Perbaikan Healthcheck Container Reverb

### Masalah:
Container `auranex-staging-reverb` berstatus `unhealthy` pada output `docker ps`.

### Penyebab:
Container Reverb menggunakan base image `dunglas/frankenphp` yang mewarisi instruksi healthcheck internal untuk memeriksa port metrics Caddy (`2019`). Karena container ini dioverride untuk hanya menjalankan `php artisan reverb:start` (bukan web server Caddy/FrankenPHP), port `2019` tidak aktif, sehingga healthcheck terus gagal.

### Solusi:
Menonaktifkan healthcheck bawaan untuk service `reverb` pada file `/var/www/stagging/auranex-erp/docker-compose.staging.yml`:
```yaml
  reverb:
    build:
      context: .
      dockerfile: Dockerfile
    image: ${IMAGE_NAME:-auranex-erp-app-staging}
    container_name: auranex-staging-reverb
    restart: unless-stopped
    working_dir: /var/www/html
    command: php artisan reverb:start --host=0.0.0.0 --port=8080
    volumes:
      - .:/var/www/html
    networks:
      - auranex-shared-network
    ports:
      - "39002:8080"
    healthcheck:
      disable: true # Menghilangkan status unhealthy bawaan FrankenPHP
```

---

## Summary Langkah Restart & Cache Clear
Jika melakukan deploy atau perubahan konfigurasi serupa di kemudian hari, jalankan langkah-langkah berikut:
1. Re-create container:
   ```bash
   docker compose -f docker-compose.staging.yml -p auranex-staging --project-directory /var/www/stagging/auranex-erp up -d
   ```
2. Bersihkan cache Laravel agar konfigurasi `.env` baru terbaca:
   ```bash
   docker exec auranex-staging-app php artisan config:clear
   docker exec auranex-staging-app php artisan cache:clear
   ```
3. Periksa status container:
   ```bash
   docker ps | grep auranex-staging
   ```
