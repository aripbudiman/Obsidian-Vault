#### Cara paling mudah: **postgres_exporter + Prometheus + Grafana**

---

### 1. Install postgres_exporter
```bash
# Download
wget https://github.com/prometheus-community/postgres_exporter/releases/latest/download/postgres_exporter-0.15.0.linux-amd64.tar.gz

tar xzf postgres_exporter-*.tar.gz
cd postgres_exporter-*
```
Buat file environment:
```bash
# /etc/postgres_exporter.env
DATA_SOURCE_NAME="postgresql://user:password@172.21.0.2:5432/postgres?sslmode=disable"
```
Jalankan:
```bash
./postgres_exporter --config.file=postgres_exporter.env
# Default jalan di port 9187
```
### 2. Config Prometheus
```bash
# prometheus.yml
scrape_configs:
  - job_name: 'postgresql'
    static_configs:
      - targets: ['localhost:9187']
    scrape_interval: 15s
```
### 3. Grafana — Import Dashboard

Cara paling cepat, pakai dashboard yang sudah jadi:

1. Buka Grafana → **Dashboards → Import**
2. Masukkan ID: **`9628`** (PostgreSQL Database)
3. Pilih Prometheus datasource
4. Klik **Import**

Dashboard ini langsung include:

- Total connections vs max
- Idle / active / idle in transaction
- Connections per database
- Query performance
### 4. Kalau pakai Docker
```bash
# docker-compose.yml
services:
  postgres_exporter:
    image: prometheuscommunity/postgres-exporter
    environment:
      DATA_SOURCE_NAME: "postgresql://user:pass@172.21.0.2:5432/postgres?sslmode=disable"
    ports:
      - "9187:9187"

  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

```bash
docker-compose up -d
```
---
### Query Grafana yang Relevan (PromQL)

Setelah setup, bisa tambah panel custom:
```bash
# Total koneksi aktif
pg_stat_activity_count

# Idle connections
pg_stat_activity_count{state="idle"}

# Koneksi per client IP
pg_stat_activity_count by (client_addr)

# Usage % dari max_connections
pg_stat_activity_count / pg_settings_max_connections * 100
```
---
Ringkasan Alur
```bash
PostgreSQL → postgres_exporter (:9187) → Prometheus (:9090) → Grafana (:3000)
```