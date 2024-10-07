# Dasar-dasar Motoko

## Pendahuluan
Motoko adalah bahasa pemrograman yang dirancang khusus untuk Internet Computer (IC). Bahasa ini menawarkan keamanan tipe yang kuat dan model aktor yang memungkinkan paralelisme dan penanganan pesan secara asinkron.

### 1. Deklarasi Variabel
Di Motoko, variabel bisa dideklarasikan sebagai *immutable* (tidak dapat diubah) atau *mutable* (dapat diubah). Variabel yang bersifat *immutable* dideklarasikan dengan kata kunci `let`, sedangkan yang *mutable* dengan `var`.

```motoko
let x : Int = 5;   // immutable (tidak bisa diubah)
var y : Int = 10;  // mutable (bisa diubah)
```

#### Tipe Data
Motoko mendukung beberapa tipe data dasar:
- **Int**: Bilangan bulat
- **Nat**: Bilangan bulat non-negatif
- **Float**: Bilangan desimal
- **Bool**: Tipe boolean (`true` atau `false`)
- **Text**: String teks
- **Principal**: Identifikasi unik untuk canister

Contoh penggunaan:
```motoko
let nama : Text = "Internet Computer";
let angka : Int = 42;
let status : Bool = true;
```

### 2. Fungsi
Fungsi di Motoko dapat bersifat publik atau privat, tergantung apakah fungsi tersebut dapat diakses dari luar atau hanya di dalam aktor itu sendiri.

#### Deklarasi Fungsi Dasar
```motoko
public func tambah(a: Int, b: Int) : Int {
    return a + b;
}
```
- `public` menunjukkan bahwa fungsi ini dapat diakses dari luar aktor.
- `a: Int` dan `b: Int` adalah parameter dengan tipe data `Int`.
- `: Int` setelah tanda kurung menunjukkan tipe kembalian fungsi.

#### Fungsi Asinkron
Motoko mendukung operasi asinkron yang memungkinkan fungsi untuk memproses panggilan tanpa menunggu hasil langsung.

```motoko
public async func fetchData() : async Text {
    return await "Data berhasil diambil!";
}
```
- Fungsi asinkron menggunakan kata kunci `async` dan `await` untuk menangani operasi asinkron seperti panggilan jaringan.

### 3. Aktor
Aktor adalah unit eksekusi dasar dalam Motoko, yang beroperasi secara paralel dan bertanggung jawab atas pengelolaan pesan asinkron. Setiap aktor memiliki status internal yang hanya bisa dimodifikasi oleh fungsi aktor tersebut.

#### Deklarasi Aktor
```motoko
actor HelloActor {
    public func greet() : Text {
        return "Halo, Internet Computer!";
    }
}
```
- `actor` mendefinisikan blok aktor baru.
- `greet` adalah fungsi publik yang mengembalikan teks.

#### Menyimpan dan Mengambil Data di Aktor
Aktor dapat menyimpan variabel state yang bisa diakses dan dimodifikasi melalui fungsi publik.

```motoko
actor Counter {
    var count : Int = 0;

    public func increment() : Int {
        count += 1;
        return count;
    }

    public func getCount() : Int {
        return count;
    }
}
```
- `var count : Int = 0;` adalah variabel *mutable* yang menyimpan status jumlah.
- Fungsi `increment` menambah nilai `count` dan mengembalikan nilai terbaru.

### 4. Pola Asinkron dan Pesan
Motoko menggunakan model berbasis pesan, di mana aktor berkomunikasi satu sama lain dengan mengirimkan pesan secara asinkron. Ini sangat berguna dalam pengembangan aplikasi terdistribusi.

#### Mengirim Pesan antar Aktor
Aktor dapat mengirim pesan ke aktor lain untuk meminta data atau mengeksekusi fungsi secara asinkron.

```motoko
actor A {
    public func ping(other : actor { public func pong() : async Text }) : async Text {
        return await other.pong();
    }
}

actor B {
    public func pong() : async Text {
        return "Pong dari aktor B!";
    }
}
```
- `actor A` memiliki fungsi `ping` yang mengirim pesan ke aktor `B` untuk memanggil fungsi `pong`.
- Ini adalah contoh interaksi antar aktor di Motoko.

### 5. Tipe dan Koleksi Data

#### Opsi Tipe
Motoko mendukung opsi tipe untuk menangani nilai yang mungkin ada atau tidak ada. Tipe `?` digunakan untuk menunjukkan nilai yang bisa *null*.

```motoko
let maybeNumber : ?Int = null;  // tidak ada nilai (null)
let anotherNumber : ?Int = ?42;  // nilai ada (42)
```

#### Daftar (Array)
Motoko menyediakan tipe daftar yang memungkinkan penyimpanan data dalam koleksi berurutan.

```motoko
let angkaList : [Int] = [1, 2, 3, 4, 5];
```
- Daftar adalah koleksi elemen dari tipe tertentu yang didefinisikan dengan tanda kurung kotak `[]`.

#### Peta (HashMap)
Motoko juga mendukung struktur data peta (key-value pair) yang bisa digunakan untuk menyimpan pasangan data.

```motoko
import HashMap "mo:base/HashMap";

let map = HashMap.HashMap<Text, Int>();
map.put("satu", 1);
map.put("dua", 2);
```

### 6. Error Handling (Penanganan Error)
Motoko menyediakan mekanisme penanganan error menggunakan tipe `Result`.

```motoko
public func divide(a : Int, b : Int) : Result<Int, Text> {
    if (b == 0) {
        return #err("Tidak bisa membagi dengan nol.");
    } else {
        return #ok(a / b);
    }
}
```
- `Result` digunakan untuk mengembalikan nilai sukses (`#ok`) atau error (`#err`).

### 7. Interaksi dengan Canister Lain
Aktor di Motoko dapat memanggil fungsi canister lain di Internet Computer.

#### Contoh Panggilan Canister Lain
```motoko
import Principal "mo:base/Principal";

actor Caller {
    public async func panggilCanister(canisterId: Principal) : async Text {
        let response : Text = await canisterId.greet();
        return response;
    }
}
```

### Kesimpulan
Motoko adalah bahasa yang kuat dan aman dengan fitur yang memungkinkan pengembangan aplikasi terdistribusi di atas Internet Computer. Dengan dukungan untuk tipe data eksplisit, aktor, dan mekanisme asinkron, Motoko memungkinkan pemrograman yang efisien dan skalabel di lingkungan blockchain.
