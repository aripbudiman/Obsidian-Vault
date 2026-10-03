oke bagian ini kejawab, sekarang kamu perhatikan ERD saya, disini baru menghandle sampai jurnal umum, dan sekarang ke jurnal khsuus atau transaksi khusus  
#### 1. ada modal isi formnya ada:
   - jenis transaksi (tambah  modal dan penarikan/prive)
   - tanggal
   - dari akun kalau jenisnya tambah modal, dari akun kalau jenisnya prive
   - simpan ke kalau pilihannya tambah modal, untuk akun kalau pilihannya prive
   - jumlah
   - keterangan
#### 2. ada menu penerimaan kas yang di formnya ada field:
   - tanggal
   - no referensi optional
   - simpan ke (kas & bank)
   - terima dari (pendapatan)
   - jumlah / nominal
   - keterangan
#### 3. ada menu pengeluaran kas yang di formnya ada field:
- tanggal
- no_referensi
- untuk keperluan (beban/persediaan/lainnya)
- dikeluarkan dari (kas & bank)
- jumlah/nominal
- keterangan
#### 4. menu piutang yang di formnya ada field:
- tanggal
- jatuh tempo
- no ref (optional)
- keterangan yang di kanan nya ada icon kontak yang bisa ambil dari master kontak
- untuk piutang (akun piutang)
- diambil dari (kas & bank / pendapatan)
- jumlah
#### 5. menu hutang yang di formnya ada field:
- tanggal
- jatuh tempo
- no ref
- keterangan yang di kanannya ada kontak yang bisa ambil dari master kontak
- simpan ke (penerimaan asset/beban)
- dari (akun hutang)
- jumlah /nominal
coba kamu perhatikan di jurnal transaksi khusus ini polanya hampir mirip sih dan saling berkaitan, ketika user input tambah modal maka row nya akan muncul di menu penerimaan kas karena itu uang masuk dari luar, begitupun yang lainnya. menurut mu transaksi khusus ini dibuat table baru kah ketika user buat akan store ke table transaksi khusus sekaligus di post juga ke jurnal umum atau table jurnal_entries