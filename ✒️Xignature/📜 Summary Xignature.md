**1. Pengecekan Pengguna (****Check User****)**

- Klien mengecek status pendaftaran pengguna terlebih dahulu menggunakan **NIK** atau **XignatureID** melalui endpoint `GET /auth/check/:identity`.
- Jika pengguna sudah terdaftar dan terverifikasi, sistem mengembalikan status terdaftar. Jika belum terdaftar, alur dilanjutkan ke registrasi.

---

**2. Registrasi Pengguna (****User Registration****)**

- Klien mendaftarkan pengguna baru via `POST /auth/registration` dengan menyertakan data pribadi (nama lengkap, NIK, email, nomor telepon, tanggal lahir) serta berkas identitas berupa **foto selfie (base64)** dan **foto KTP (base64)**.
- Pendaftaran dan verifikasi ini wajib dilakukan untuk memvalidasi identitas penandatangan sebelum sertifikat elektronik dan pasangan kunci (_key pair_) diterbitkan.

---

**3. Aktivasi Pengguna (****User Activation****)**

- Setelah registrasi berhasil dikirim, pengguna menerima email aktivasi atau klien dapat memanggil endpoint `GET /auth/activate/:token` menggunakan token yang didapat dari respons registrasi.
- Setelah token divalidasi dan aktivasi berhasil, sertifikat elektronik pengguna resmi diterbitkan.

---

**4. Pembuatan & Aktivasi PAT (****Generate & Activate Personal Access Token****)**

- Klien meminta pembuatan PAT untuk pengguna via `POST /auth/generate_pat` dengan mengirimkan `xignatureId` dan tanggal kedaluwarsa token.
- Pengguna (penandatangan) harus mengonfirmasi dan mengaktifkan PAT tersebut melalui tautan yang dikirimkan ke alamat email terdaftar.
- Status keaktifan PAT dapat dipastikan melalui endpoint `GET /auth/check_pat/:token`.

---

**5. Penandatanganan Dokumen (****Sign Document****)**

Xignature menggunakan pendekatan **Remote Signing (Hash-Only)**, di mana dokumen asli tetap berada di sisi klien demi menjaga kerahasiaan data. Penandatanganan dapat dilakukan melalui dua pilihan alur:

- **Pilihan A: Direct API (****POST /sign/hash****)**
    1. Klien membuat nilai _hash digest_ dari berkas PDF.
    2. Klien mengirimkan string _hash_ (base64) ke endpoint `POST /sign/hash` dengan menyertakan `api-key` dan `pat` pada HTTP header.
    3. Xignature merespons dengan nilai _signed hash_.
    4. Klien menyematkan (_stamp_) kembali _signed hash_ tersebut ke dalam berkas PDF asli.
- **Pilihan B: Sign Gateway (Docker Lokal -** **POST /stamp/document****)**
    1. Klien mengirimkan dokumen PDF (base64), posisi tanda tangan (_signPositions_), dan token PAT ke _Sign Gateway_ lokal yang berjalan di port `1303`.
    2. _Sign Gateway_ secara otomatis mengolah pembuatan _hash_, meminta _signed hash_ ke server Xignature, dan menempelkan kembali tanda tangan digital ke PDF hingga mengembalikan berkas PDF utuh yang telah ditandatangani.

```bash
sequenceDiagram
    autonumber
    actor Client as Client / Application
    participant GW as Sign Gateway (Docker :1303)
    participant API as Xignature API Server
    participant HSM as Xignature HSM Core

    rect rgb(232, 234, 246)
    note over Client, HSM: METODE A: Direct API (POST /sign/hash)
    Client->>Client: 1. Generate Hash Digest (Base64) dari PDF
    Client->>API: 2. POST /sign/hash (Headers: api-key, pat | Body: hash)
    API->>HSM: 3. Verifikasi Credentials & Kirim Hash ke HSM
    HSM-->>API: 4. Sign Hash menggunakan Private Key User
    API-->>Client: 5. Response 200 OK (Signed Hash Base64)
    Client->>Client: 6. Embed / Stamp Signed Hash ke Dokumen PDF Asli
    end

    rect rgb(243, 229, 245)
    note over Client, HSM: METODE B: Sign Gateway (Docker Local POST /stamp/document)
    Client->>GW: 1. POST /stamp/document (Body: PDF Base64, pat, signPositions, signatureImage)
    GW->>GW: 2. Generate Hash Digest secara Lokal
    GW->>API: 3. Request Sign Hash (Gunakan ENV API_KEY & PAT User)
    API->>HSM: 4. Process Sign Hash via HSM
    HSM-->>GW: 5. Return Signed Hash
    GW->>GW: 6. Auto-Stamp Signed Hash, Gambar TTD & QR Code ke PDF
    GW-->>Client: 7. Response 200 OK (Signed PDF Document Base64)
    end

```