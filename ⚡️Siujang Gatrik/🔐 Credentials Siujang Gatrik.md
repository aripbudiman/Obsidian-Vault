Berikut adalah ringkasan kredensial dan lingkungan (_environment_) Staging (Dev) serta Production untuk API SIUJANG GATRIK:

**1. Mekanisme Kredensial & Autentikasi (Header HTTP)**

- Setiap pemanggilan _web service_ mewajibkan dua variabel utama pada HTTP Header:
    - **X-Consumer-Id**: Kode unik pemanggil/consumer.
    - **X-Signature**: Token otentikasi JWT dengan algoritma **HS256**, ber-header `{"alg":"HS256","typ":"JWT"}` dan payload `{"iat": timestamp}` (menggunakan timezone `Asia/Jakarta` / GMT+7).
- **Consumer Secret Key** disimpan secara rahasia di sisi _client_ untuk men-_generate_ `X-Signature` dan **tidak pernah dikirimkan** melalui HTTP Request.

---

**2. Environment Staging / Development (Dev)**

- **Base URL / Domain Endpoint**:
    - `https://dev-ujang.artristik.co.id/`
- **Contoh Kredensial Dev (Dokumen v3.0)**:
    - **Consumer ID**: `serlindo-dev`
    - **Consumer Secret Key**: `TGc6jixn9giTv80`
    - _(Pada_ _sample code_ _penguji juga tercantum sampel_ _sample_ _dan_ _Secret123__)._

---

**3. Environment Production (Prod)**

- **Base URL / Domain Endpoint**:
    - `https://siujang.esdm.go.id/`
- **Kredensial Prod**:
    - **Consumer ID** dan **Consumer Secret Key** resmi untuk lingkungan Production **diberikan langsung oleh DJK** (Direktorat Jenderal Ketenagalistrikan) dan tidak dicantumkan di dokumen publik