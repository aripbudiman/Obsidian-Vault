## Rancangan Database Schema
Tambahkan beberapa kolom/tabel baru pada Laravel untuk menyimpan status kredensial dan audit trail penandatanganan:
### A. Modifikasi Tabel `users`
```
ALTER TABLE users ADD COLUMN xignature_id VARCHAR(100) NULL AFTER email;
ALTER TABLE users ADD COLUMN xignature_pat TEXT NULL AFTER xignature_id; -- Encrypted
ALTER TABLE users ADD COLUMN pat_status ENUM('active', 'expired', 'unregistered') D
```
### **B. Modifikasi Tabel** **invoices**
