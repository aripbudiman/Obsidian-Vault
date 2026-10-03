### Struktur Flutter vs HTML

Flutter pakai **widget tree** mirip HTML yang pakai tag bersarang, tapi bedanya semua "elemen" di Flutter adalah objek Dart.
#### Perbandingan Konsep

|HTML|Flutter|
|---|---|
|`<div>`|`Container` / `Column` / `Row`|
|`<p>`|`Text`|
|`<button>`|`ElevatedButton` / `FloatingActionButton`|
|`style=""`|Parameter langsung di widget (misal `color:`, `fontSize:`)|
|DOM tree|Widget tree|
#### Breakdown Kode Kamu

**1. Entry point**

```dart
void main() {
  runApp(const MyApp()); // seperti index.html di-load browser
}
```
**2. `StatelessWidget` vs `StatefulWidget`**

```dart
// StatelessWidget = tampilan statis, tidak ada data yang berubah
class MyApp extends StatelessWidget { ... }

// StatefulWidget = punya "state" yang bisa berubah dan trigger re-render
// mirip React component dengan useState
class MyHomePage extends StatefulWidget { ... }
```

**3. Widget tree = nested "tag"**

```dart
// Ini seperti:
// <scaffold>
//   <appbar>...</appbar>
//   <body><center><column>...</column></center></body>
// </scaffold>

Scaffold(
  appBar: AppBar(...),
  body: Center(
    child: Column(
      children: [ Text(...), Text(...) ],
    ),
  ),
)
```

**4. `setState()` = trigger re-render**

```dart
void _incrementCounter() {
  setState(() {
    _counter++; // ubah state → Flutter otomatis rebuild UI
  });
}
// mirip useState setter di React
```

---
### Widget Flutter (Padanan "Tag" HTML)

Flutter tidak punya "tag" seperti HTML, tapi punya **widget**. Berikut yang paling sering dipakai:

---

#### 📦 Layout

|Widget|Fungsi|Mirip HTML|
|---|---|---|
|`Container`|Kotak serbaguna, bisa dikasih warna, padding, margin|`<div>`|
|`Column`|Susun widget secara **vertikal**|`<div style="flex-direction: column">`|
|`Row`|Susun widget secara **horizontal**|`<div style="flex-direction: row">`|
|`Stack`|Tumpuk widget di atas satu sama lain|`<div style="position: relative">`|
|`Expanded`|Isi sisa ruang yang tersedia|`flex: 1` di CSS|
|`SizedBox`|Beri jarak atau ukuran tetap|`<div style="width/height">`|
|`Padding`|Tambah padding di sekitar widget|`padding` di CSS|
|`Center`|Tengahkan widget|`margin: auto` di CSS|
|`Wrap`|Seperti Row tapi otomatis pindah baris|`flex-wrap: wrap`|

---

#### ✏️ Teks & Gambar

|Widget|Fungsi|
|---|---|
|`Text`|Tampilkan teks|
|`RichText`|Teks dengan style berbeda-beda dalam satu baris|
|`Image.network`|Tampilkan gambar dari URL|
|`Image.asset`|Tampilkan gambar dari folder project|
|`Icon`|Tampilkan icon Material|

---

#### 🖱️ Tombol & Input

|Widget|Fungsi|Mirip HTML|
|---|---|---|
|`ElevatedButton`|Tombol dengan bayangan|`<button>`|
|`TextButton`|Tombol flat tanpa bayangan|`<button>`|
|`IconButton`|Tombol berupa icon|`<button><icon></button>`|
|`FloatingActionButton`|Tombol bulat mengambang|-|
|`TextField`|Input teks|`<input type="text">`|
|`Checkbox`|Centang|`<input type="checkbox">`|
|`Switch`|Toggle on/off|`<input type="checkbox">`|
|`Slider`|Geser nilai|`<input type="range">`|
|`DropdownButton`|Pilihan dropdown|`<select>`|

---

#### 📜 Scroll & List

|Widget|Fungsi|Mirip HTML|
|---|---|---|
|`SingleChildScrollView`|Buat konten bisa di-scroll|`overflow: scroll`|
|`ListView`|List panjang yang bisa di-scroll|`<ul>` / `<ol>`|
|`ListView.builder`|List dinamis dari data|`.map()` di React|
|`GridView`|Tampilan grid|CSS Grid|
|`PageView`|Swipe antar halaman|-|

---

#### 🏗️ Struktur Halaman

|Widget|Fungsi|
|---|---|
|`Scaffold`|Kerangka halaman (appbar, body, FAB, drawer)|
|`AppBar`|Navbar atas|
|`BottomNavigationBar`|Navbar bawah|
|`Drawer`|Menu hamburger dari samping|
|`TabBar`|Tab navigasi|
|`AlertDialog`|Popup dialog|
|`SnackBar`|Notifikasi kecil di bawah|

---

#### 🎨 Dekorasi & Efek

|Widget|Fungsi|
|---|---|
|`Card`|Kotak dengan shadow dan rounded corner|
|`ClipRRect`|Potong widget jadi rounded|
|`Opacity`|Atur transparansi|
|`DecoratedBox`|Tambah dekorasi (gradient, border, shadow)|
|`CircleAvatar`|Gambar/icon berbentuk lingkaran|

---

#### 🔀 Navigasi

|Widget|Fungsi|
|---|---|
|`Navigator.push`|Pindah ke halaman baru|
|`Navigator.pop`|Kembali ke halaman sebelumnya|
|`MaterialPageRoute`|Definisi halaman tujuan|
### Tidak Semua Widget Bisa Pakai Child/Children

Setiap widget sudah **ditentukan dari sananya** mau pakai `child`, `children`, atau tidak sama sekali.

---

#### 1. Widget dengan `child` (hanya 1 anak)

Widget-widget ini memang dirancang hanya untuk **membungkus 1 widget:**

dart

```dart
Container(child: ...)
Center(child: ...)
Padding(child: ...)
Align(child: ...)
Expanded(child: ...)
SizedBox(child: ...)
Card(child: ...)
ClipRRect(child: ...)
Scaffold(body: ...)      // namanya beda tapi konsep sama = 1 widget
AppBar(title: ...)       // sama
```

---

#### 2. Widget dengan `children` (banyak anak)

Widget-widget ini memang dirancang untuk **menampung banyak widget:**

dart

```dart
Column(children: [...])
Row(children: [...])
Stack(children: [...])
ListView(children: [...])
Wrap(children: [...])
```

---

#### 3. Widget tanpa child sama sekali

Widget ini **tidak bisa punya anak** karena dia adalah konten itu sendiri:

dart

```dart
Text('Halo')             // sudah final, tidak bisa dibungkus lagi dari dalam
Icon(Icons.star)
Image.network('url')
Divider()
CircularProgressIndicator()
Slider(...)
Switch(...)
TextField(...)
```

---

#### Cara Tau Widget Itu Pakai Yang Mana?

Cukup lihat dari **fungsinya:**

|Jenis Widget|Pakai|
|---|---|
|Widget posisi/dekorasi|`child`|
|Widget layout multi-item|`children`|
|Widget konten/data|tidak ada|

Atau di VS Code/Android Studio, tinggal **hover** widget-nya dan lihat dokumentasinya — langsung ketahuan parameternya apa saja 😄

---

#### Contoh Salah vs Benar

dart

```dart
// ❌ Salah - Text tidak punya child
Text(
  'Halo',
  child: Icon(Icons.star),   // ERROR!
)

// ❌ Salah - Center tidak punya children
Center(
  children: [               // ERROR!
    Text('A'),
    Text('B'),
  ],
)

// ✅ Benar - mau 2 widget di tengah? bungkus dulu pakai Column
Center(
  child: Column(            // Column yang handle banyak widget
    children: [
      Text('A'),
      Text('B'),
    ],
  ),
)
```
