## Variable dart
### String
variable string untuk menyimpan nilai teks
ex: 
```dart
String name='Arip budiman';
print(name);
```
### int
varian `int` untuk menyimpan nilai angka atau number
ex:
```dart
int age=26;
print(age);
```
### double
double unutk menyimpan angka dalam bentuk `1.65` 
ex:
```dart
double height=1.72;
print(height);
```
### bool
variable `bool` untuk menyimpan true atau false
ex:
```dart
bool isActive=false;
print(isActive);
```
### List
kumpulan data (array)
ex:
```dart
List<String> nama = ["Andi", "Budi", "Cici"];
print(nama);
```
### Map
data key-value (seperti object JS)
ex:
```dart
Map<String, dynamic> user = {"name": "Andi", "age": 20};
print(user);
```
### dynamic
tipe bebas, bisa apa saja
ex:
```dart
dynamic apaSaja="Halo";
prnt(apaSaja);
```
### var
tipe otomatis ditebak dart
ex:
```dart
var nama='andi';
print(nama);
```
### final
nilai tidak bisa diubah setelah diisi
ex:
```dart
final String nama = "Andi";
print(nama);
```
### const
nilai tidak bisa diubah & sudah ditentukan saat compile
ex:
```dart
const int maxUser = 100;
print(maxUser);
```
### Bedanya `var`, `final`, `const`
Ini yang sering bikin bingung, worth ditambah di notes kamu:
```md
var   → bisa diubah nilainya
final → tidak bisa diubah setelah pertama diisi
const → tidak bisa diubah, nilainya harus sudah pasti saat coding

ex:
var x = 10;
x = 20;        // ✅ boleh

final y = 10;
y = 20;        // ❌ error

const z = 10;
z = 20;        // ❌ error
```
## Function Dart
### void
ini tidak mengembalikan apapun
ex:
```dart
void sayHello() { print("Halo"); }

print(sayHello());
```
### int, String, bool, dll → return sesuai tipenya
ex: 
```dart 
int tambah(int a, int b) { return a + b; } 

String sayHello(){ return "Selamat Pagi"};
```
### Future
return nilai tapi async (menunggu)
ex:
```dart 
Future fetchNama() async { return "Andi"; } 
```
### Arrow Function
function singkat satu baris
ex:
```dart
int tambah(int a, int b) => a + b;
```
### Named parameter
parameter pakai nama, urutan bebas
ex:
```dart
void biodata({required String nama, required int umur}) { print("$nama, $umur tahun"); } 
// cara panggil: 
biodata(umur: 20, nama: "Andi");
```
### Optional parameter
parameter boleh tidak diisi
ex:
 ```dart 
 void sapa(String nama, [String? sapaan]) { print("${sapaan ?? "Halo"} $nama"); } 
 // cara panggil: 
 sapa("Andi"); // Halo Andi 
 sapa("Andi", "Hai"); // Hai Andi 
 ```
 
 ## Control Flow
### if / else
ex:
```dart
int nilai = 80;

if (nilai >= 80) {
  print("Lulus");
} else if (nilai >= 60) {
  print("Cukup");
} else {
  print("Gagal");
}
```

### switch
ex:
```dart
String hari = "Senin";

switch (hari) {
  case "Senin":
    print("Awal minggu");
    break;
  case "Jumat":
    print("Akhir minggu");
    break;
  default:
    print("Hari biasa");
}
```

### for
ex:
```dart
for (int i = 0; i < 5; i++) {
  print(i);
}
```

### for in → loop list
ex:
```dart
List nama = ["Andi", "Budi", "Cici"];

for (var n in nama) {
  print(n);
}
```

### forEach → loop list pakai callback
ex:
```dart
nama.forEach((n) => print(n));
```

### while → loop selama kondisi true
ex:
```dart
int i = 0;

while (i < 5) {
  print(i);
  i++;
}
```

### break & continue
ex:
```dart
for (int i = 0; i < 5; i++) {
  if (i == 3) break;    // berhenti total
  if (i == 1) continue; // skip, lanjut iterasi berikutnya
  print(i);
}
// output: 0, 2
```

### Ternary → if/else singkat satu baris
ex:
```dart
int nilai = 80;
String hasil = nilai >= 80 ? "Lulus" : "Gagal";
print(hasil); // Lulus
```
## OOP (Object Oriented Programming)

### Class & Object → blueprint untuk membuat object
ex:
```dart
class User {
  String name;
  int age;

  User(this.name, this.age); // constructor
}

// cara pakai:
var user = User("Andi", 20);
print(user.name); // Andi
print(user.age);  // 20
```

### Named Constructor → constructor lebih dari satu
ex:
```dart
class User {
  String name;
  int age;

  User(this.name, this.age);

  // constructor tambahan
  User.guest() {
    name = "Guest";
    age = 0;
  }
}

// cara pakai:
var tamu = User.guest();
print(tamu.name); // Guest
```

### Factory Constructor → buat object dari Map (JSON)
ex:
```dart
class User {
  String name;
  String email;

  User({required this.name, required this.email});

  factory User.fromJson(Map<String, dynamic> json) {
    return User(
      name: json['name'],
      email: json['email'],
    );
  }
}

// cara pakai:
var data = {"name": "Andi", "email": "andi@gmail.com"};
var user = User.fromJson(data);
print(user.name); // Andi
```

### Extends → class turunan (inheritance)
ex:
```dart
class Hewan {
  String nama;
  Hewan(this.nama);

  void suara() {
    print("...");
  }
}

class Kucing extends Hewan {
  Kucing(String nama) : super(nama); // panggil constructor parent

  @override
  void suara() {
    print("Meow!");
  }
}

// cara pakai:
var kucing = Kucing("Kitty");
print(kucing.nama); // Kitty
kucing.suara();     // Meow!
```

### Abstract Class → class template, tidak bisa dibuat objectnya
ex:
```dart
abstract class Shape {
  double luas(); // wajib diimplementasi oleh turunannya
}

class Lingkaran extends Shape {
  double radius;
  Lingkaran(this.radius);

  @override
  double luas() {
    return 3.14 * radius * radius;
  }
}

// cara pakai:
var lingkaran = Lingkaran(7);
print(lingkaran.luas()); // 153.86
```

### getter & setter → akses dan ubah property
ex:
```dart
class User {
  String _name; // underscore = private
  User(this._name);

  String get name => _name;           // getter
  set name(String value) => _name = value; // setter
}

// cara pakai:
var user = User("Andi");
print(user.name);  // Andi (getter)
user.name = "Budi"; // setter
print(user.name);  // Budi
```
## Null Safety

### ? → variabel boleh null
ex:
```dart
String? nama;       // boleh null
print(nama);        // null

nama = "Andi";
print(nama);        // Andi
```

### ! → paksa anggap tidak null (hati-hati!)
ex:
```dart
String? nama = "Andi";
print(nama!.length); // 4

String? kosong;
print(kosong!.length); // ❌ ERROR! nilainya null
```

### ?? → nilai default kalau null
ex:
```dart
String? nama;
print(nama ?? "Guest"); // Guest (karena nama null)

nama = "Andi";
print(nama ?? "Guest"); // Andi (karena nama sudah ada isinya)
```

### ??= → isi nilai kalau masih null
ex:
```dart
String? nama;
nama ??= "Guest"; // isi "Guest" kalau nama masih null
print(nama);      // Guest
```

### ?. → akses property kalau tidak null
ex:
```dart
String? nama;
print(nama?.length); // null (tidak error, karena pakai ?.)

nama = "Andi";
print(nama?.length); // 4
```

### late → deklarasi dulu, isi nanti (dijamin tidak null)
ex:
```dart
late String nama;

// diisi sebelum dipakai
nama = "Andi";
print(nama); // Andi

// kalau diakses sebelum diisi → ❌ ERROR!
```
## Collection

### List → kumpulan data berurutan (array)
ex:
```dart
List<String> nama = ["Andi", "Budi", "Cici"];

nama.add("Dodi");         // tambah item
nama.remove("Andi");      // hapus item
nama[0];                  // akses index
nama.length;              // panjang list
nama.isEmpty;             // cek kosong
nama.contains("Budi");    // cek ada atau tidak
```

### Map → data key-value (object JS)
ex:
```dart
Map<String, dynamic> user = {
  "name": "Andi",
  "age": 20,
};

user["name"];             // akses value
user["email"] = "a@a.com" // tambah key baru
user.keys;                // semua key
user.values;              // semua value
user.containsKey("name"); // cek key ada atau tidak
```

### Set → list tapi tidak boleh duplikat
ex:
```dart
Set<String> hobi = {"coding", "gaming", "coding"};
print(hobi); // {coding, gaming} → duplikat otomatis dihapus
```

### where → filter list (seperti .filter() JS)
ex:
```dart
List<int> angka = [1, 2, 3, 4, 5];
var genap = angka.where((n) => n % 2 == 0).toList();
print(genap); // [2, 4]
```

### map → ubah tiap item list (seperti .map() JS)
ex:
```dart
List<String> nama = ["andi", "budi"];
var kapital = nama.map((n) => n.toUpperCase()).toList();
print(kapital); // [ANDI, BUDI]
```

### reduce → gabungkan semua item jadi satu nilai
ex:
```dart
List<int> angka = [1, 2, 3, 4, 5];
var total = angka.reduce((a, b) => a + b);
print(total); // 15
```

### spread operator → gabung list
ex:
```dart
List<String> a = ["Andi", "Budi"];
List<String> b = ["Cici", "Dodi"];
List<String> semua = [...a, ...b];
print(semua); // [Andi, Budi, Cici, Dodi]
```

## Async / Await

### Future → nilai yang akan datang di masa depan
ex:
```dart
Future<String> fetchNama() async {
  return "Andi";
}

void main() async {
  String nama = await fetchNama();
  print(nama); // Andi
}
```

### async/await → tunggu proses selesai dulu
ex:
```dart
Future<void> fetchData() async {
  print("Mulai fetch...");
  await Future.delayed(Duration(seconds: 2)); // simulasi loading
  print("Data berhasil dimuat!");
}

void main() async {
  await fetchData();
  print("Selesai");
}
// output:
// Mulai fetch...
// (jeda 2 detik)
// Data berhasil dimuat!
// Selesai
```

### then → alternatif await tanpa async
ex:
```dart
fetchNama().then((nama) {
  print(nama); // Andi
});
```

### try/catch → tangkap error saat async
ex:
```dart
Future<void> fetchData() async {
  try {
    final response = await http.get(Uri.parse('https://api.example.com'));
    print(response.body);
  } catch (e) {
    print("Error: $e");
  }
}
```

### Stream → seperti Future tapi datanya bisa berkali-kali
ex:
```dart
Stream<int> countdown() async* {
  for (int i = 3; i >= 0; i--) {
    await Future.delayed(Duration(seconds: 1));
    yield i; // kirim nilai satu per satu
  }
}

void main() async {
  await for (var n in countdown()) {
    print(n); // 3, 2, 1, 0 (tiap 1 detik)
  }
}
```