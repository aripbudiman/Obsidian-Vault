### Urutan Belajar Flutter yang Terstruktur

---

#### Stage 1 — Fondasi Dart dulu (1-2 minggu)

Flutter pakai bahasa Dart, kalau skip ini bakal bingung sintaxnya

dart

```dart
// Yang wajib paham:
- Variable & tipe data (String, int, double, bool, List, Map)
- Function & arrow function
- Class & OOP dasar (constructor, extends)
- Null safety (tanda ? dan !)
- async/await (nanti butuh untuk API)
```

> Kalau udah familiar JavaScript/Java, Dart bakal cepet nyangkutnya

---

#### Stage 2 — Widget Dasar (1-2 minggu)

Ini yang lagi kamu pelajari sekarang ✅

```
- Stateless vs Stateful widget
- Layout: Container, Column, Row, Stack
- Konten: Text, Image, Icon
- Input: TextField, Button
- Child vs Children
- Style: BoxDecoration, TextStyle, EdgeInsets
```

---

#### Stage 3 — State Management (2-3 minggu)

Cara widget "berkomunikasi" dan update data

```
- setState (paling dasar)
- Provider (recommended untuk pemula)
- Riverpod (lebih modern, banyak dipakai industri)
```

---

#### Stage 4 — Navigasi (1 minggu)

Pindah antar halaman

```
- Navigator.push & pop
- Named routes
- GoRouter (recommended)
```

---

#### Stage 5 — Koneksi API (2-3 minggu)

Ambil data dari internet

```
- HTTP package
- async/await & Future
- JSON parsing
- FutureBuilder & StreamBuilder
```

---

#### Stage 6 — Penyimpanan Data (1-2 minggu)

```
- SharedPreferences (simpan data kecil)
- SQLite / Hive (database lokal)
```

---

#### Stage 7 — Fitur Tambahan (ongoing)

```
- Firebase (auth, database, storage)
- Push notification
- Maps & GPS
- Kamera & galeri
```

---

#### Jalur Singkatnya

```
Dart Dasar → Widget & Layout → State → Navigasi → API → Storage
```

|TypeScript|Dart|
|---|---|
|`string`, `number`, `boolean`|`String`, `int`, `bool`|
|`any`|`dynamic`|
|`interface` / `type`|`class`|
|`async/await`|`async/await`|
|`Array<string>` / `string[]`|`List<String>`|
|`Promise<string>`|`Future<String>`|
|`?: optional`|`String?` null safety|

---

Jadi otak kamu sebenernya udah **"terlatih"** berpikir strongly typed, tinggal ganti sintaxnya aja ke Dart.

Beda utamanya cuma:

- Dart pakai **class** untuk segalanya, tidak ada `interface`
- Dart punya **widget tree** untuk UI (ini yang beda dari TS)
- Dart `null safety` lebih strict dari TS