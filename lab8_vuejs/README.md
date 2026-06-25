# 🌐 Lab 8 – Frontend SPA Secured (Vue.js)

> **Praktikum Pemrograman Web Lanjut**
> Nama: Den Fahmi Satria | NIM: 312410523 | Universitas Pelita Bangsa – Teknik Informatika

---

## 📌 Deskripsi

Repository ini adalah **Frontend Single Page Application (SPA)** yang dibangun dengan **Vue.js 3** (CDN). Aplikasi ini bekerja berpasangan dengan backend REST API CodeIgniter 4 ([lab11_ci4](https://github.com/)) sebagai penyedia data.

Fitur utama:
- ✅ Sistem Login dengan JWT-style Token (Bearer Token via Axios)
- ✅ Navigation Guard (Proteksi Rute berbasis status login)
- ✅ Axios Interceptors (Penyuntikan token otomatis ke setiap request)
- ✅ CRUD Artikel (Create, Read, Update, Delete) melalui API
- ✅ SPA Routing dengan Vue Router 4 (Hash History)

---

## 🛠️ Teknologi yang Digunakan

| Teknologi | Versi | Fungsi |
|---|---|---|
| **Vue.js 3** | `3.x` (CDN) | Framework JavaScript utama untuk membangun UI reaktif |
| **Vue Router 4** | `4.x` (CDN) | Mengatur navigasi antar halaman (SPA Routing) |
| **Axios** | Latest (CDN) | HTTP Client untuk berkomunikasi dengan REST API backend |
| **HTML5** | – | Struktur halaman web |
| **CSS3** | – | Styling antarmuka pengguna |

---

## 📁 Struktur Direktori

```
lab8_vuejs/
├── index.html                  # Entry point utama aplikasi SPA
└── assets/
    ├── css/
    │   └── style.css           # Styling global aplikasi
    └── js/
        ├── app.js              # Inisialisasi Vue, Router, Axios Interceptors
        └── components/
            ├── Home.js         # Halaman Beranda
            ├── Login.js        # Halaman Login (komunikasi ke API /api/login)
            ├── Artikel.js      # Halaman CRUD Artikel (komunikasi ke API /post)
            └── About.js        # Halaman Profil Mahasiswa
```

---

## ⚙️ Cara Menjalankan

### Prasyarat
- Backend CI4 sudah berjalan di `http://localhost:8080`
- XAMPP aktif (Apache)

### Langkah-langkah

**1. Clone repository ini:**
```bash
git clone https://github.com/<username>/lab8_vuejs.git
```

**2. Letakkan folder di dalam direktori htdocs XAMPP:**
```
C:\xampp\htdocs\lab8_vuejs\
```

**3. Pastikan backend (lab11_ci4) berjalan:**
```bash
# Masuk ke folder backend CI4
cd C:\xampp\htdocs\lab11_ci4\ci4

# Jalankan server CI4 di port 8080
php spark serve --port 8080
```

**4. Buka browser dan akses:**
```
http://localhost/lab8_vuejs/index.html
```

---

## 🔐 Informasi Login

Gunakan kredensial user yang sudah ada di database CI4 (`lab_ci4`):

| Field | Value |
|---|---|
| **Username** | `admin` (sesuai data di tabel `user`) |
| **Password** | (sesuai data di tabel `user`) |

---

## 🔗 Endpoint API yang Digunakan

| Method | Endpoint | Deskripsi | Auth |
|---|---|---|---|
| `POST` | `/api/login` | Login & mendapatkan token | ❌ Tidak perlu |
| `GET` | `/post` | Menampilkan semua artikel | ❌ Tidak perlu |
| `POST` | `/post` | Menambah artikel baru | ✅ Bearer Token |
| `PUT` | `/post/{id}` | Mengubah artikel | ✅ Bearer Token |
| `DELETE` | `/post/{id}` | Menghapus artikel | ✅ Bearer Token |

---

## 🗺️ Alur Kerja Aplikasi

```
[User buka index.html]
       ↓
[Vue Router memuat komponen sesuai URL]
       ↓
[Navigation Guard cek localStorage "isLoggedIn"]
       ↓ (jika rute dilindungi & belum login)
[Redirect ke /login]
       ↓ (user isi form & submit)
[Axios POST ke /api/login (CI4 Backend)]
       ↓ (berhasil)
[Token disimpan di localStorage]
       ↓
[Axios Interceptor otomatis inject token ke setiap request]
       ↓
[User dapat mengakses CRUD Artikel]
```

---

## 📸 Halaman Aplikasi

| Halaman | Rute | Deskripsi |
|---|---|---|
| Beranda | `#/` | Halaman welcome publik |
| Login | `#/login` | Form autentikasi |
| Kelola Artikel | `#/artikel` | Tabel CRUD data artikel (login required) |
| About Me | `#/about` | Profil mahasiswa (login required) |

---

## 📎 Repository Backend (CI4)

Backend REST API untuk project ini tersedia di:
👉 **[lab11_ci4 – CodeIgniter 4 REST API](https://github.com/<username>/lab11_ci4)**

---

*Dibuat sebagai bagian dari tugas Praktikum Pemrograman Web – Universitas Pelita Bangsa*
