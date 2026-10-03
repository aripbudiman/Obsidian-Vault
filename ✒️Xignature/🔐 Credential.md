**1. Kredensial & Autentikasi yang Dibutuhkan**

- **API Key (****api-key****)**: Kredensial utama yang diterbitkan oleh Xignature. Wajib disertakan di dalam **HTTP Header** untuk setiap panggilan API (`api-key: API-KEY klien`).
- **Personal Access Token (PAT)**: Token otorisasi khusus pengguna/penanda tangan (_signer_) yang digunakan saat melakukan proses penandatanganan _hash_ maupun _document stamping_. PAT ini diaktifkan oleh pengguna melalui tautan email.
- **Kredensial Docker Registry**: Digunakan jika klien menggunakan _Sign Gateway_ mandiri:
    - **User**: `public`
    - **Password**: `CS0BkvzAytj9yxj`
    - **Command**: `docker login registry.xignature.co.id -u public -p CS0BkvzAytj9yxj`
- **Environment Variables (ENV)**: Diperlukan saat menjalankan _Sign Gateway_ di Docker, meliputi `API_KEY` dan `API_URL` (`https://sandbox.xignature.co.id` atau `https://api.xignature.co.id`).
**2. Tata Cara Penggunaan & Alur Integrasi API**

1. **Pengecekan & Registrasi Pengguna (****Signer****)**:
    - **Pengecekan Status**: Lakukan pengecekan status pengguna via `GET /auth/check/:identity` menggunakan NIK atau XignatureID.
    - **Registrasi Pengguna**: Jika belum terdaftar, daftarkan pengguna via `POST /auth/registration` dengan menyertakan data pribadi, foto selfie (base64), dan foto KTP (base64).
    - **Aktivasi**: Pengguna mengaktifkan akun via tautan di email atau melalui endpoint `/auth/activate/:token`.
2. **Pembuatan & Aktivasi PAT (****Personal Access Token****)**:
    - Minta pembuatan PAT via `POST /auth/generate_pat` dengan menyertakan `xignatureId` dan `expirationDate`.
    - Pengguna mengonfirmasi aktivasi PAT melalui tautan email.
    - Cek status keaktifan PAT via `GET /auth/check_pat/:token`.
3. **Penandatanganan Dokumen**:
    - **Metode Direct API (****POST /sign/hash****)**: Kirim nilai _hash digest_ dari dokumen PDF (base64) dengan header `pat` dan `api-key`. Xignature mengembalikan _signed hash_ untuk disematkan kembali pada dokumen PDF asli.
    - **Metode Sign Gateway (Docker)**: Klien memasang container Docker Sign Gateway di server sendiri. Penandatanganan dilakukan secara lokal melalui `POST /stamp/document` ke `http://localhost:1303/stamp/document`.
4. **Pembubuhan E-Meterai (Opsional)**:
    - Jika memerlukan e-meterai, panggil `POST /emeterai/generate_sn` dengan header `pat` dan `api-key` untuk membuat _Serial Number_ (SN) sebelum proses pembubuhan dilakukan.
**Catatan Base URL:**

- **Sandbox:** `https://sandbox.xignature.co.id`
- **Production:** `https://api.xignature.co.id`
- **Sign Gateway (Docker Lokal):** `http://localhost:1303`