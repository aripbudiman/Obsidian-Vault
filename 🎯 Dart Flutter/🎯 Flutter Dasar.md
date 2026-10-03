## Flutter Dasar

### Struktur Minimal Flutter
```dart
import 'package:flutter/material.dart'; // wajib import ini

void main() {
  runApp(const MyApp()); // entry point, jalankan app
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: Text("Halo Flutter!"),
        ),
      ),
    );
  }
}
```
---
#### Penjelasan Tiap Bagian
```md
import material.dart  → library UI flutter (wajib)
main()               → entry point seperti index.html
runApp()             → jalankan widget pertama
StatelessWidget      → widget tanpa state
build()              → return tampilan UI
MaterialApp          → root app, pengaturan global
Scaffold             → kerangka halaman
body                 → isi halaman
```
#### AppBar Default (biasa)


```dart
AppBar(
  title: Text("Setor Tabungan"),
)
```

---

#### AppBar Custom (seperti di gambar kamu)

```dart
AppBar(
  backgroundColor: Colors.white,
  elevation: 0, // hilangkan shadow bawah
  leading: IconButton( // tombol panah kiri
    icon: Icon(Icons.arrow_back, color: Colors.blue),
    onPressed: () => Navigator.pop(context),
  ),
  title: Text(
    "Setor Tabungan",
    style: TextStyle(color: Colors.blue),
  ),
  actions: [ // widget di sebelah kanan
    Padding(
      padding: EdgeInsets.only(right: 16),
      child: CircleAvatar(
        backgroundImage: NetworkImage("url_foto_profile"),
      ),
    ),
  ],
)
```

---

#### Breakdown Parameter AppBar

|Parameter|Fungsi|
|---|---|
|`leading`|widget paling kiri (biasanya tombol back)|
|`title`|judul di tengah|
|`actions`|list widget di kanan|
|`backgroundColor`|warna background appbar|
|`elevation`|shadow di bawah appbar|
## Detail Widget
### MaterialApp
```dart
MaterialApp(
  // === WAJIB / PALING SERING DIPAKAI ===
  home: Scaffold(),                  // halaman utama saat app dibuka
  title: 'Nama App',                 // nama app (muncul di task switcher)

  // === TEMA ===
  theme: ThemeData(                  // tema light mode
    colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
    fontFamily: 'Poppins',
  ),
  darkTheme: ThemeData.dark(),       // tema dark mode
  themeMode: ThemeMode.system,       // ikuti sistem / light / dark

  // === NAVIGASI ===
  routes: {                          // daftar semua halaman
    '/': (context) => HomePage(),
    '/profile': (context) => ProfilePage(),
  },
  initialRoute: '/',                 // halaman pertama yang dibuka
  onGenerateRoute: (settings) {},    // handle route dinamis

  // === LOCALE / BAHASA ===
  locale: Locale('id', 'ID'),        // set bahasa Indonesia
  supportedLocales: [
    Locale('id', 'ID'),
    Locale('en', 'US'),
  ],

  // === LAINNYA ===
  debugShowCheckedModeBanner: false, // hilangkan banner "DEBUG" di pojok
)
```
Yang **paling sering dipakai** sehari-hari:
```
home                        → halaman utama
title                       → nama app
theme                       → warna & font global
debugShowCheckedModeBanner  → hilangkan label debug
routes                      → navigasi antar halaman
```
### Scaffold
```dart
Scaffold(
  appBar: AppBar(
    title: Text('Halaman Utama'),
  ),
  body: Center(
    child: Text('Ini adalah konten utama (body)'),
  ),
  floatingActionButton: FloatingActionButton(
    onPressed: () {},
    child: Icon(Icons.add),
  ),
  drawer: Drawer(
    child: Center(child: Text('Menu Samping')),
  ),
  bottomNavigationBar: BottomNavigationBar(
    items: [
      BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
      BottomNavigationBarItem(icon: Icon(Icons.person), label: 'Profil'),
    ],
  ),
)
```
### Container
```dart
Container(
  margin: EdgeInsets.all(20.0),      // Jarak luar kotak
  padding: EdgeInsets.all(16.0),     // Jarak dalam kotak ke teks
  width: 200.0,
  height: 100.0,
  alignment: Alignment.center,       // Teks akan berada di tengah kotak
  decoration: BoxDecoration(
    color: Colors.blue,              // Warna kotak
    borderRadius: BorderRadius.circular(12.0), // Sudut melengkung
  ),
  child: Text(
    'Halo Flutter!',
    style: TextStyle(color: Colors.white),
  ),
)
```
### Center
```dart
Center(
  widthFactor: 2.0,    // lebar center = lebar child x widthFactor
  heightFactor: 2.0,   // tinggi center = tinggi child x heightFactor
  child: Text("Tengah"),
)
```
### Padding

```dart
Padding(
  padding: EdgeInsets.all(16),                        // semua sisi
  padding: EdgeInsets.only(top: 8, left: 16),         // sisi tertentu
  padding: EdgeInsets.symmetric(vertical: 8, horizontal: 16), // pasangan sisi
  child: Text("Ada Padding"),
)
```
### SizedBox
```dart
SizedBox(
  width: 200,              // lebar tetap
  height: 100,             // tinggi tetap
  child: Text("SizedBox"), // opsional, bisa tanpa child untuk spacer

  // shortcut untuk spacer
  SizedBox(height: 16),    // jarak vertikal
  SizedBox(width: 16),     // jarak horizontal
  SizedBox.expand(),       // isi semua ruang yang tersedia
  SizedBox.shrink(),       // ukuran sekecil mungkin
)
```
### Column
```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,      // posisi vertikal
  // MainAxisAlignment.start       → atas
  // MainAxisAlignment.end         → bawah
  // MainAxisAlignment.center      → tengah
  // MainAxisAlignment.spaceBetween → jarak rata antar item
  // MainAxisAlignment.spaceAround  → jarak rata + setengah di tepi
  // MainAxisAlignment.spaceEvenly  → jarak rata termasuk tepi

  crossAxisAlignment: CrossAxisAlignment.center,    // posisi horizontal
  // CrossAxisAlignment.start      → kiri
  // CrossAxisAlignment.end        → kanan
  // CrossAxisAlignment.center     → tengah
  // CrossAxisAlignment.stretch    → lebar penuh

  mainAxisSize: MainAxisSize.max,  // max = ambil semua ruang, min = sesuai isi
  spacing: 16,                     // jarak antar children (Flutter 3.27+)
  children: [
    Text("Atas"),
    Text("Tengah"),
    Text("Bawah"),
  ],
)
```
### Row
```dart
Row(
  mainAxisAlignment: MainAxisAlignment.center,      // posisi horizontal
  // sama seperti Column tapi arahnya horizontal

  crossAxisAlignment: CrossAxisAlignment.center,    // posisi vertikal
  // sama seperti Column tapi arahnya vertikal

  mainAxisSize: MainAxisSize.max,
  spacing: 16,                     // jarak antar children (Flutter 3.27+)
  children: [
    Text("Kiri"),
    Text("Tengah"),
    Text("Kanan"),
  ],
)
```
### Stack
```dart
Stack(
  alignment: Alignment.center,     // posisi default semua children
  // Alignment.topLeft, topCenter, topRight
  // Alignment.centerLeft, center, centerRight
  // Alignment.bottomLeft, bottomCenter, bottomRight

  fit: StackFit.loose,             // loose = sesuai ukuran child, expand = isi semua
  clipBehavior: Clip.hardEdge,     // potong child yang keluar batas stack
  children: [
    Container(color: Colors.blue), // paling bawah
    Text("Tumpuk"),                // di atasnya

    // pakai Positioned untuk posisi spesifik
    Positioned(
      top: 10,
      right: 10,
      child: Icon(Icons.star),
    ),
  ],
)
```
### Expanded
```dart
Row(
  children: [
    Expanded(
      flex: 2,           // ambil 2 bagian dari total ruang
      child: Container(color: Colors.blue),
    ),
    Expanded(
      flex: 1,           // ambil 1 bagian dari total ruang
      child: Container(color: Colors.red),
    ),
  ],
)
```
### Wrap
```dart
Wrap(
  direction: Axis.horizontal,        // arah susun (horizontal / vertikal)
  alignment: WrapAlignment.start,    // posisi horizontal
  spacing: 8,                        // jarak antar item horizontal
  runSpacing: 8,                     // jarak antar baris
  children: [
    Chip(label: Text("Flutter")),
    Chip(label: Text("Dart")),
    Chip(label: Text("Mobile")),
    Chip(label: Text("Development")),
  ],
)
```
### ListView
```dart
ListView(
  scrollDirection: Axis.vertical,    // arah scroll (vertical / horizontal)
  reverse: false,                    // balik urutan scroll
  padding: EdgeInsets.all(16),       // padding seluruh list
  physics: BouncingScrollPhysics(),  // efek scroll (bounce / clamp / never)
  shrinkWrap: false,                 // true = ukuran sesuai isi
  children: [
    Text("Item 1"),
    Text("Item 2"),
  ],
)

// ListView.builder → untuk data dinamis / banyak
ListView.builder(
  itemCount: 10,                     // jumlah item
  itemBuilder: (context, index) {    // build tiap item
    return Text("Item $index");
  },
)

// ListView.separated → seperti builder + ada separator
ListView.separated(
  itemCount: 10,
  separatorBuilder: (context, index) => Divider(), // pemisah antar item
  itemBuilder: (context, index) {
    return Text("Item $index");
  },
)
```
### GridView
```dart
GridView.count(
  crossAxisCount: 2,                 // jumlah kolom
  crossAxisSpacing: 8,               // jarak antar kolom
  mainAxisSpacing: 8,                // jarak antar baris
  padding: EdgeInsets.all(16),
  childAspectRatio: 1.0,             // rasio lebar:tinggi tiap item
  children: [
    Container(color: Colors.blue),
    Container(color: Colors.red),
  ],
)

// GridView.builder → untuk data dinamis
GridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
    crossAxisCount: 2,
    crossAxisSpacing: 8,
    mainAxisSpacing: 8,
  ),
  itemCount: 10,
  itemBuilder: (context, index) {
    return Container(color: Colors.blue);
  },
)
```
### SingleChildScrollView
```dart
SingleChildScrollView(
  scrollDirection: Axis.vertical,    // arah scroll (vertical / horizontal)
  reverse: false,                    // balik arah scroll
  padding: EdgeInsets.all(16),       // padding konten
  physics: BouncingScrollPhysics(),  // efek scroll
  child: Column(                     // biasanya isinya Column
    children: [
      Text("Konten panjang..."),
      Text("Konten panjang..."),
      Text("Konten panjang..."),
    ],
  ),
)
```
### Text
```dart
Text(
  "Halo Flutter",
  style: TextStyle(
    fontSize: 16,                        // ukuran font
    fontWeight: FontWeight.bold,         // tebal
    // FontWeight.w100 - w900, bold, normal
    fontStyle: FontStyle.italic,         // miring
    color: Colors.black,                 // warna teks
    letterSpacing: 2.0,                  // jarak antar huruf
    wordSpacing: 4.0,                    // jarak antar kata
    decoration: TextDecoration.underline,// garis bawah
    // TextDecoration.lineThrough       → coret
    // TextDecoration.none              → tanpa dekorasi
    fontFamily: 'Poppins',              // font custom
    height: 1.5,                        // line height
  ),
  textAlign: TextAlign.center,          // rata teks
  // TextAlign.left, right, center, justify
  maxLines: 2,                          // batas baris
  overflow: TextOverflow.ellipsis,      // potong dengan "..."
  // TextOverflow.clip, fade, visible
  softWrap: true,                       // otomatis pindah baris
)
```
### RichText
```dart
RichText(
  text: TextSpan(
    style: TextStyle(color: Colors.black),  // style default
    children: [
      TextSpan(text: "Halo "),
      TextSpan(
        text: "Flutter",
        style: TextStyle(
          color: Colors.blue,
          fontWeight: FontWeight.bold,
        ),
      ),
      TextSpan(text: " Keren!"),
    ],
  ),
)
```
### Icon
```dart
Icon(
  Icons.star,                  // icon material
  size: 24,                    // ukuran
  color: Colors.amber,         // warna
  semanticLabel: 'Bintang',    // label accessibility
)
```
### Image
```dart
// dari internet
Image.network(
  'https://example.com/foto.jpg',
  width: 200,
  height: 200,
  fit: BoxFit.cover,           // cara isi ruang
  // BoxFit.contain            → isi tanpa crop
  // BoxFit.cover              → isi penuh, crop jika perlu
  // BoxFit.fill               → isi penuh, stretch
  // BoxFit.fitWidth           → sesuai lebar
  // BoxFit.fitHeight          → sesuai tinggi
  loadingBuilder: (context, child, progress) {
    if (progress == null) return child;
    return CircularProgressIndicator(); // tampil saat loading
  },
  errorBuilder: (context, error, stack) {
    return Icon(Icons.broken_image);   // tampil saat error
  },
)

// dari asset lokal
Image.asset(
  'assets/images/foto.jpg',
  width: 200,
  height: 200,
  fit: BoxFit.cover,
)

// dari file (hasil ambil dari galeri)
Image.file(
  File('path/to/file'),
  width: 200,
  height: 200,
  fit: BoxFit.cover,
)
```
### CircleAvatar
```dart
CircleAvatar(
  radius: 40,                           // ukuran lingkaran
  backgroundColor: Colors.blue,         // warna background
  backgroundImage: NetworkImage('url'), // gambar dari internet
  // backgroundImage: AssetImage('assets/foto.jpg') // dari asset

  child: Text("A"),                     // tampil kalau tidak ada gambar
)
```
### TextField
```dart
TextField(
  controller: TextEditingController(), // kontrol value dari luar
  keyboardType: TextInputType.text,    // jenis keyboard
  // TextInputType.number             → angka
  // TextInputType.email              → email
  // TextInputType.phone              → nomor HP
  // TextInputType.multiline          → banyak baris
  obscureText: false,                  // true = sembunyikan teks (password)
  maxLines: 1,                         // batas baris
  maxLength: 100,                      // batas karakter
  enabled: true,                       // false = tidak bisa diisi
  autofocus: false,                    // langsung fokus saat tampil
  onChanged: (value) {                 // dipanggil setiap ketikan
    print(value);
  },
  onSubmitted: (value) {               // dipanggil saat enter
    print(value);
  },
  decoration: InputDecoration(
    hintText: "Masukan nama...",       // placeholder
    labelText: "Nama",                 // label di atas field
    prefixIcon: Icon(Icons.person),    // icon di kiri
    suffixIcon: Icon(Icons.clear),     // icon di kanan
    border: OutlineInputBorder(        // border kotak
      borderRadius: BorderRadius.circular(8),
    ),
    filled: true,                      // aktifkan warna background
    fillColor: Colors.grey[100],       // warna background
    errorText: "Nama wajib diisi",     // pesan error
  ),
)
```
### ElevatedButton
```dart
ElevatedButton(
  onPressed: () {},                    // null = tombol disabled
  onLongPress: () {},                  // tahan lama
  style: ElevatedButton.styleFrom(
    backgroundColor: Colors.blue,      // warna background
    foregroundColor: Colors.white,     // warna teks & icon
    minimumSize: Size(200, 50),        // ukuran minimum
    maximumSize: Size(300, 60),        // ukuran maximum
    padding: EdgeInsets.symmetric(horizontal: 24, vertical: 12),
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(8),
    ),
    elevation: 4,                      // shadow
    shadowColor: Colors.black,         // warna shadow
  ),
  child: Text("Tombol"),
)
```
### TextButton
```dart
TextButton(
  onPressed: () {},
  style: TextButton.styleFrom(
    foregroundColor: Colors.blue,      // warna teks
    padding: EdgeInsets.all(16),
  ),
  child: Text("Klik Saya"),
)
```
### OutlinedButton
```dart
OutlinedButton(
  onPressed: () {},
  style: OutlinedButton.styleFrom(
    foregroundColor: Colors.blue,
    side: BorderSide(               // style border
      color: Colors.blue,
      width: 2,
    ),
    shape: RoundedRectangleBorder(
      borderRadius: BorderRadius.circular(8),
    ),
  ),
  child: Text("Outlined"),
)
```
### IconButton
```dart
IconButton(
  onPressed: () {},
  icon: Icon(Icons.favorite),
  iconSize: 24,                     // ukuran icon
  color: Colors.red,                // warna icon
  tooltip: "Favorit",               // teks saat ditahan
  padding: EdgeInsets.all(8),
)
```
### FloatingActionButton
```dart
FloatingActionButton(
  onPressed: () {},
  backgroundColor: Colors.blue,
  foregroundColor: Colors.white,
  elevation: 6,
  tooltip: "Tambah",
  mini: false,                      // true = ukuran kecil
  child: Icon(Icons.add),
)

// Extended FAB (dengan teks)
FloatingActionButton.extended(
  onPressed: () {},
  icon: Icon(Icons.add),
  label: Text("Tambah"),
  backgroundColor: Colors.blue,
)
```
### Checkbox
```dart
Checkbox(
  value: true,                      // status centang
  onChanged: (value) {              // dipanggil saat diubah
    print(value);
  },
  activeColor: Colors.blue,         // warna saat centang
  checkColor: Colors.white,         // warna tanda centang
)
```
### Switch
```dart
Switch(
  value: true,                      // status on/off
  onChanged: (value) {
    print(value);
  },
  activeColor: Colors.blue,         // warna saat on
  inactiveThumbColor: Colors.grey,  // warna bulatan saat off
)
```
### Slider
```dart
Slider(
  value: 0.5,                       // nilai saat ini (0.0 - 1.0)
  min: 0.0,                         // nilai minimum
  max: 100.0,                       // nilai maximum
  divisions: 10,                    // jumlah pembagian
  label: "50",                      // label saat digeser
  activeColor: Colors.blue,         // warna bagian aktif
  onChanged: (value) {
    print(value);
  },
)
```
### DropdownButton
```dart
DropdownButton<String>(
  value: "Pilihan 1",               // nilai yang dipilih
  hint: Text("Pilih salah satu"),   // placeholder
  isExpanded: true,                 // lebar penuh
  icon: Icon(Icons.arrow_drop_down),
  underline: SizedBox(),            // hilangkan garis bawah
  onChanged: (value) {
    print(value);
  },
  items: ["Pilihan 1", "Pilihan 2", "Pilihan 3"]
    .map((item) => DropdownMenuItem(
      value: item,
      child: Text(item),
    ))
    .toList(),
)
```
### Card
```dart
Card(
  color: Colors.white,              // warna background
  elevation: 4,                     // shadow
  shadowColor: Colors.black,        // warna shadow
  shape: RoundedRectangleBorder(
    borderRadius: BorderRadius.circular(12),
  ),
  margin: EdgeInsets.all(8),        // jarak luar
  child: Padding(
    padding: EdgeInsets.all(16),
    child: Text("Isi Card"),
  ),
)
```
### Divider
```dart
Divider(
  height: 1,                        // tinggi area divider
  thickness: 1,                     // tebal garis
  color: Colors.grey,               // warna garis
  indent: 16,                       // jarak dari kiri
  endIndent: 16,                    // jarak dari kanan
)
```
### CircularProgressIndicator
```dart
CircularProgressIndicator(
  color: Colors.blue,               // warna
  backgroundColor: Colors.grey,     // warna track
  strokeWidth: 4,                   // tebal garis
  value: 0.7,                       // 0.0-1.0, null = animasi terus
)
```
### SnackBar
```dart
// cara tampilkan
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(
    content: Text("Berhasil disimpan!"),
    duration: Duration(seconds: 3),    // lama tampil
    backgroundColor: Colors.green,     // warna background
    behavior: SnackBarBehavior.floating, // mengambang / fixed
    action: SnackBarAction(            // tombol aksi
      label: "Undo",
      onPressed: () {},
    ),
  ),
)
```
### AlertDialog
```dart
// cara tampilkan
showDialog(
  context: context,
  builder: (context) => AlertDialog(
    title: Text("Konfirmasi"),
    content: Text("Apakah kamu yakin?"),
    actions: [
      TextButton(
        onPressed: () => Navigator.pop(context), // tutup dialog
        child: Text("Batal"),
      ),
      ElevatedButton(
        onPressed: () {},
        child: Text("Ya"),
      ),
    ],
  ),
)
```