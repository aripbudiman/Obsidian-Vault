#### Solusi Arsitektur Database:

Tambahkan kolom `current_balance` langsung di tabel `chart_of_accounts`:

```sql
  chart_of_accounts
  -----------------
  id                  PK
  account_code        string
  account_name        string
  account_type_id     FK
  parent_id           FK
  is_header           boolean
  account_class       enum
  is_contra           boolean
+ current_balance     decimal (Default: 0.00) -- KOLOM CACHE SALDO
```
#### Cara Kerjanya di Backend (Event Trigger):

1. Saat Jurnal Umum statusnya berubah menjadi **`Posted`**: Jalankan fungsi _background update_ untuk meng-update kolom `current_balance` pada akun yang terlibat:
    - _Jika akun bertambah (sesuai saldo normal):_ `UPDATE chart_of_accounts SET current_balance = current_balance + :amount WHERE id = :account_id`
    - _Jika akun berkurang:_ `UPDATE chart_of_accounts SET current_balance = current_balance - :amount WHERE id = :account_id`
2. Saat Jurnal Umum statusnya diubah menjadi **`Void`** atau **`Cancelled`**: Jalankan fungsi pembalik untuk memulihkan saldo di kolom `current_balance`.

#### Hasilnya:

- Ketika user ingin melihat saldo saat ini di UI: Sistem hanya perlu melakukan `SELECT current_balance FROM chart_of_accounts WHERE id = X` yang merupakan query **O(1) (Sangat Instan!)**.
- Tidak ada kalkulasi berulang yang berat dari transaksi awal tahun. Database kamu tetap ringan dan responsif.