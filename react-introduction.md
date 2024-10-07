# Pengantar React

## Pendahuluan
React adalah pustaka JavaScript untuk membangun antarmuka pengguna (UI). React sangat populer karena kemampuannya dalam membuat aplikasi berbasis komponen yang dapat digunakan kembali dan efisien dalam merender perubahan secara dinamis.

React dikembangkan oleh Facebook dan dirancang untuk memudahkan pengelolaan antarmuka pengguna yang kompleks dengan membagi UI menjadi komponen-komponen kecil yang modular dan mudah dipelihara.

### 1. Komponen di React
Di React, setiap bagian dari UI merupakan sebuah *komponen*. Komponen dapat berupa elemen kecil seperti tombol, atau bahkan seluruh halaman.

#### Deklarasi Komponen dengan Fungsi
Komponen di React biasanya dibuat dengan fungsi:
```jsx
function Halo() {
    return <h1>Halo, Dunia!</h1>;
}
```
Komponen di atas adalah komponen paling sederhana yang menampilkan teks "Halo, Dunia!". Komponen ini memanfaatkan sintaks JSX yang memungkinkan Anda menulis HTML dalam JavaScript.

#### Deklarasi Komponen dengan Kelas
Selain komponen fungsi, Anda juga dapat membuat komponen dengan kelas:
```jsx
import React from 'react';

class Halo extends React.Component {
    render() {
        return <h1>Halo, Dunia!</h1>;
    }
}
```
Komponen berbasis kelas digunakan sebelum React Hooks diperkenalkan, namun sekarang lebih umum menggunakan komponen fungsi.

### 2. Props (Properties)
`Props` adalah cara untuk mengirim data dari satu komponen ke komponen lain. Props tidak dapat diubah oleh komponen penerima (bersifat read-only).

#### Mengirim Data dengan Props
```jsx
function Welcome(props) {
    return <h1>Selamat Datang, {props.nama}</h1>;
}

function App() {
    return <Welcome nama="Ulul" />;
}
```
Pada contoh di atas, `App` mengirimkan props `nama` ke komponen `Welcome`. Props ini dapat digunakan di dalam `Welcome` untuk menampilkan teks dinamis.

#### Default Props
Anda juga bisa memberikan nilai default untuk props:
```jsx
function Welcome(props) {
    return <h1>Selamat Datang, {props.nama || "Pengguna"}</h1>;
}
```
Jika `props.nama` tidak diberikan, maka secara otomatis akan menggunakan nilai default "Pengguna".

### 3. State di React
`State` adalah cara untuk menyimpan data lokal yang dimiliki dan dikelola oleh suatu komponen. Tidak seperti `props`, `state` dapat diubah di dalam komponen.

#### Menggunakan State dengan React Hooks
React Hooks memungkinkan komponen fungsi untuk menggunakan state. Hook paling umum untuk state adalah `useState`.

```jsx
import React, { useState } from 'react';

function Penghitung() {
    const [jumlah, setJumlah] = useState(0);

    return (
        <div>
            <p>Jumlah: {jumlah}</p>
            <button onClick={() => setJumlah(jumlah + 1)}>Tambah</button>
        </div>
    );
}
```
- `useState(0)` mendeklarasikan variabel `jumlah` dengan nilai awal 0.
- `setJumlah` adalah fungsi untuk memperbarui nilai `jumlah`.
- Setiap kali `setJumlah` dipanggil, komponen akan di-*render* ulang dengan nilai terbaru.

#### State di Komponen Kelas
Jika menggunakan komponen berbasis kelas, Anda bisa menggunakan state seperti ini:
```jsx
class Penghitung extends React.Component {
    constructor(props) {
        super(props);
        this.state = { jumlah: 0 };
    }

    tambah = () => {
        this.setState({ jumlah: this.state.jumlah + 1 });
    }

    render() {
        return (
            <div>
                <p>Jumlah: {this.state.jumlah}</p>
                <button onClick={this.tambah}>Tambah</button>
            </div>
        );
    }
}
```

### 4. Pengelolaan Lifecycle Komponen
Komponen React memiliki siklus hidup (lifecycle) yang terdiri dari tiga fase utama:
- **Mounting**: Komponen sedang dibuat dan ditambahkan ke DOM.
- **Updating**: Komponen mengalami perubahan, baik karena perubahan props atau state.
- **Unmounting**: Komponen dihapus dari DOM.

#### Hooks untuk Mengelola Lifecycle
React menyediakan hook `useEffect` untuk mengelola efek samping dalam komponen fungsi. Hook ini memungkinkan kita untuk menjalankan kode pada fase tertentu dari siklus hidup komponen.

```jsx
import React, { useState, useEffect } from 'react';

function FetchData() {
    const [data, setData] = useState(null);

    useEffect(() => {
        fetch('https://api.example.com/data')
            .then(response => response.json())
            .then(data => setData(data));
    }, []);

    return (
        <div>
            {data ? <p>Data: {data}</p> : <p>Memuat...</p>}
        </div>
    );
}
```
- `useEffect` dipanggil setelah komponen di-*render* untuk pertama kali.
- Argumen kedua (`[]`) menunjukkan bahwa `useEffect` hanya dipanggil sekali saat komponen di-*mount*.

### 5. Event Handling (Penanganan Event)
React menggunakan sintaks khusus untuk menangani event, mirip dengan event handler di HTML tetapi dalam bentuk camelCase.

#### Penanganan Event
```jsx
function Tombol() {
    function klikHandler() {
        alert('Tombol diklik!');
    }

    return <button onClick={klikHandler}>Klik Saya</button>;
}
```
Pada contoh di atas, event `onClick` dikaitkan dengan fungsi `klikHandler` yang akan dijalankan saat tombol diklik.

### 6. Render Bersyarat
Terkadang Anda perlu merender komponen atau elemen UI berdasarkan kondisi tertentu. React memungkinkan Anda untuk melakukan render bersyarat menggunakan ekspresi JavaScript biasa seperti `if` atau `ternary operator`.

#### Render Menggunakan `if`
```jsx
function Pesan(props) {
    const sudahLogin = props.sudahLogin;
    if (sudahLogin) {
        return <h1>Selamat Datang Kembali!</h1>;
    }
    return <h1>Silakan Login</h1>;
}
```

#### Render Menggunakan Operator Ternary
```jsx
function Pesan(props) {
    return props.sudahLogin ? <h1>Selamat Datang Kembali!</h1> : <h1>Silakan Login</h1>;
}
```

### 7. Mengulang dan Merender Daftar Elemen
Jika Anda memiliki daftar data, Anda bisa menggunakan `map()` untuk merender setiap elemen dalam daftar.

```jsx
function DaftarAngka(props) {
    const angka = props.angka;
    const daftarItems = angka.map((angka) =>
        <li key={angka.toString()}>{angka}</li>
    );
    return <ul>{daftarItems}</ul>;
}

const angka = [1, 2, 3, 4, 5];
ReactDOM.render(<DaftarAngka angka={angka} />, document.getElementById('root'));
```
- Kunci (`key`) diperlukan agar React dapat mengidentifikasi elemen mana yang berubah, ditambahkan, atau dihapus.

### 8. Forms di React
Formulir di React dikelola melalui state. Dengan mengikat input form ke state, Anda dapat mengontrol dan menangani perubahan input pengguna secara dinamis.

#### Form dengan Input Terkontrol
```jsx
function FormNama() {
    const [nama, setNama] = useState('');

    function handleChange(event) {
        setNama(event.target.value);
    }

    function handleSubmit(event) {
        alert('Nama yang dikirim: ' + nama);
        event.preventDefault();
    }

    return (
        <form onSubmit={handleSubmit}>
            <label>
                Nama:
                <input type="text" value={nama} onChange={handleChange} />
            </label>
            <input type="submit" value="Kirim" />
        </form>
    );
}
```
- `value` dari input dikaitkan dengan state `nama`, dan setiap perubahan pada input akan memperbarui state menggunakan `setNama`.

### 9. Mengelola Global State dengan Context API
Context API di React memungkinkan komponen untuk berbagi state tanpa harus meneruskan props secara eksplisit di setiap tingkat komponen.

#### Membuat Context
```jsx
const TemaContext = React.createContext('terang');

function App() {
    return (
        <TemaContext.Provider value="gelap">
            <Toolbar />
        </TemaContext.Provider>
    );
}

function Toolbar() {
    return (
        <div>
            <TombolTema />
        </div>
    );
}

function TombolTema() {
    return (
        <TemaContext.Consumer>
            {tema => <button>Tema Saat Ini: {tema}</button>}
        </TemaContext.Consumer>
    );
}
```
- Dengan `TemaContext`, komponen `TombolTema` dapat mengakses nilai `tema` tanpa harus menerimanya sebagai props dari `App`.

### Kesimpulan
React adalah pustaka yang fleksibel dan efisien untuk membangun UI modern dengan komponen yang dapat digunakan kembali. Dengan menggunakan konsep seperti `props`, `state`, event handling, dan hooks, React memudahkan pengelolaan aplikasi yang kompleks.
