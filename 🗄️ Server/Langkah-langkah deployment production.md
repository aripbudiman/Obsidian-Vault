```bash
git pull origin development

docker exec auranex-production-app composer install --no-dev --optimize-autoloader


# Migrasi Database Utama (Central)
docker exec auranex-production-app php artisan migrate --force

# Migrasi Database Tenant (jika menggunakan Stancl/Tenancy)
docker exec auranex-production-app php artisan tenants:migrate


# Install package frontend
docker exec auranex-production-app yarn install

# Kompilasi aset (CSS, JS, Vue components) untuk Production
docker exec auranex-production-app yarn build

# Ubah ownership folder ke www-data
docker exec auranex-production-app chown -R www-data:www-data storage bootstrap/cache public/build

# Ubah permission agar folder dapat ditulis/dibaca
docker exec auranex-production-app chmod -R 775 storage bootstrap/cache public/build

# Bersihkan cache lama
docker exec auranex-production-app php artisan optimize:clear

# Buat cache konfigurasi dan rute baru
docker exec auranex-production-app php artisan optimize

```