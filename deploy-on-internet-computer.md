# Panduan Lengkap Deploy Aplikasi ke Internet Computer (IC)

## Pendahuluan
Internet Computer (IC) adalah jaringan terdesentralisasi yang dirancang untuk menjalankan aplikasi berbasis web tanpa perlu menggunakan infrastruktur terpusat seperti server tradisional. Pada Internet Computer, Anda dapat membangun dan mendistribusikan aplikasi terdesentralisasi (dApps) yang berjalan di atas protokol blockchain.

Panduan ini akan memberikan langkah-langkah detail tentang cara mendepoloy aplikasi ke Internet Computer menggunakan Development Framework (DFX).

### Persiapan Awal

Sebelum memulai, pastikan Anda telah memenuhi persyaratan berikut:
- Sudah terpasang **Node.js** (minimal versi 12).
- Telah menginstal **DFX SDK** (versi yang digunakan di sini adalah 0.23.0).
- Memiliki **wallet ICP** untuk membayar siklus yang diperlukan selama deployment.
- Telah menginstal **Git** (opsional, untuk pengelolaan versi kode).
  
#### 1. Instalasi DFX SDK
DFX adalah SDK (Software Development Kit) yang digunakan untuk membangun dan menjalankan aplikasi di Internet Computer.

Untuk menginstal DFX SDK, jalankan perintah berikut di terminal:
```bash
sh -ci "$(curl -fsSL https://internetcomputer.org/install.sh)"
```

Ini akan menginstal versi terbaru dari DFX. Pastikan proses instalasi selesai tanpa kesalahan. Anda bisa memverifikasi instalasi DFX dengan menjalankan:
```bash
dfx --version
```

#### 2. Membuat Proyek Baru
Setelah DFX terinstal, langkah selanjutnya adalah membuat proyek baru. Untuk memulai proyek, jalankan perintah berikut:
```bash
dfx new myapp
cd myapp
```
Ini akan membuat proyek dengan struktur direktori dasar. Anda dapat memodifikasi dan menambahkan kode sesuai kebutuhan proyek Anda.

#### 3. Memahami Struktur Proyek
Struktur proyek `myapp` yang dibuat oleh DFX terlihat seperti ini:

```bash
myapp/
├── src/
│   ├── myapp_backend/
│   │   └── main.mo        # Kode backend (Motoko)
│   └── myapp_frontend/
│       ├── src/
│       │   └── index.js    # Kode frontend (React/JavaScript)
│       └── index.html
├── dfx.json               # Konfigurasi DFX
├── package.json           # Dependencies untuk frontend
```

- **`myapp_backend`**: Ini adalah bagian backend aplikasi, ditulis dalam bahasa Motoko. Anda juga dapat menggunakan Rust jika diinginkan.
- **`myapp_frontend`**: Ini adalah bagian frontend, biasanya ditulis dengan JavaScript atau framework seperti React.
- **`dfx.json`**: File konfigurasi utama untuk proyek, berisi informasi tentang canister dan siklus.
  
#### 4. Menjalankan Proyek Secara Lokal
Sebelum melakukan deployment ke jaringan IC, Anda dapat menguji aplikasi secara lokal. Untuk melakukannya, jalankan perintah berikut:
```bash
dfx start --background
```
Ini akan memulai jaringan IC lokal di mesin Anda.

Kemudian, deploy canister backend dan frontend dengan perintah:
```bash
dfx deploy
```
Anda bisa mengakses aplikasi frontend di `http://localhost:8000` setelah deploy selesai.

### Menyiapkan Deployment ke Internet Computer

Untuk deployment ke Internet Computer, Anda perlu mengikuti beberapa langkah tambahan dibandingkan dengan menjalankan aplikasi secara lokal.

#### 5. Membuat Identitas
Internet Computer menggunakan sistem identitas untuk memverifikasi pengguna dan pengembang. Anda dapat membuat identitas baru atau menggunakan identitas yang sudah ada.

Untuk membuat identitas baru, jalankan:
```bash
dfx identity new <nama-identitas>
```
Ini akan membuat identitas lokal baru di sistem Anda. Untuk menggunakan identitas tersebut, jalankan perintah berikut:
```bash
dfx identity use <nama-identitas>
```

#### 6. Membuat Wallet
Setiap deployment di Internet Computer memerlukan siklus sebagai biaya operasi. Anda dapat membuat wallet ICP yang akan digunakan untuk membayar siklus.

Untuk membuat wallet baru:
```bash
dfx wallet --network ic create
```

Anda perlu mengisi wallet Anda dengan ICP sebelum dapat menggunakannya untuk membayar siklus.

#### 7. Menyiapkan Siklus
Sebelum dapat mendepoloy aplikasi ke IC, Anda perlu memastikan bahwa wallet Anda memiliki cukup siklus. Untuk memeriksa saldo siklus, gunakan perintah berikut:
```bash
dfx wallet --network ic balance
```

Jika saldo wallet Anda tidak mencukupi, Anda harus mengisi ICP ke wallet tersebut. Anda bisa membeli ICP dari bursa kripto atau melalui layanan penyedia siklus seperti [Cycles Wallet](https://faucet.dfinity.org).

#### 8. Konfigurasi DFX untuk Deployment
Sebelum deploy ke jaringan IC, Anda harus mengubah file `dfx.json` untuk mendefinisikan canister yang akan digunakan. Contoh file `dfx.json`:

```json
{
  "canisters": {
    "myapp_backend": {
      "main": "src/myapp_backend/main.mo",
      "type": "motoko"
    },
    "myapp_frontend": {
      "main": "src/myapp_frontend",
      "dependencies": [
        "myapp_backend"
      ],
      "type": "assets"
    }
  },
  "networks": {
    "ic": {
      "providers": [
        "https://ic0.app"
      ],
      "type": "persistent"
    }
  }
}
```

- **`networks`**: Bagian ini menunjukkan bahwa kita akan melakukan deploy ke IC melalui jaringan `https://ic0.app`.
  
#### 9. Deploy Aplikasi ke Internet Computer
Sekarang, aplikasi Anda siap untuk di-deploy ke Internet Computer. Pastikan DFX dijalankan dengan menggunakan jaringan IC:
```bash
dfx deploy --network ic
```

Perintah ini akan memulai proses deployment. Canister backend dan frontend akan di-deploy ke jaringan IC. Proses ini mungkin memerlukan beberapa menit, tergantung pada jaringan dan ukuran aplikasi Anda.

Setelah deploy selesai, Anda akan mendapatkan URL yang dapat diakses oleh siapa saja di jaringan Internet Computer. URL ini akan terlihat seperti berikut:
```bash
https://<canister-id>.ic0.app
```

#### 10. Memantau Canister dan Siklus
Setelah aplikasi Anda dideploy, Anda bisa memonitor penggunaan siklus oleh canister. Untuk melihat status canister Anda, jalankan:
```bash
dfx canister --network ic status myapp_backend
```

Perintah ini akan menampilkan informasi seperti jumlah siklus yang tersisa dan status canister di IC.

### Penyelesaian dan Pengembangan Lanjutan
Anda sekarang telah berhasil mendepoloy aplikasi ke Internet Computer. Berikut beberapa langkah pengembangan lebih lanjut:
- **Memperbarui Canister**: Jika Anda melakukan perubahan pada kode backend atau frontend, Anda dapat meng-deploy ulang canister dengan perintah `dfx deploy --network ic`.
- **Menambahkan Fitur**: Anda dapat memperluas aplikasi dengan menambahkan lebih banyak canister, seperti canister penyimpanan data, layanan autentikasi, atau integrasi blockchain lainnya.
- **Monitoring dan Analitik**: Gunakan layanan monitoring eksternal atau bawaan IC untuk melacak performa aplikasi Anda di jaringan.

### Kesimpulan
Deploy aplikasi di Internet Computer membutuhkan persiapan seperti pembuatan identitas, wallet, dan pengelolaan siklus. Proses deploy dapat dilakukan dengan DFX SDK dan file konfigurasi `dfx.json` yang mengatur canister. Setelah aplikasi ter-deploy, aplikasi dapat diakses melalui URL IC, dan Anda dapat terus memonitor penggunaan siklus dan status canister.

Internet Computer memberikan solusi yang kuat untuk membangun aplikasi terdesentralisasi dengan skalabilitas tinggi dan biaya operasional yang efisien.
