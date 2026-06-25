# Pemograman_Web2_Praktikum
# Backend REST API (CodeIgniter 4)

> **Praktikum Pemrograman Web Lanjut**
> Nama: Den Fahmi Satria | NIM: 312410500 | Universitas Pelita Bangsa – Teknik Informatika

---

## 📌 Deskripsi

Repository ini adalah **Backend REST API** yang dibangun dengan **CodeIgniter 4**. API ini menyediakan layanan data untuk aplikasi frontend Vue.js SPA serta memiliki panel admin berbasis MVC.

Fitur utama:
- ✅ REST API dengan Token-Based Authentication (Bearer Token)
- ✅ CORS Header sudah dikonfigurasi untuk komunikasi lintas origin
- ✅ Filter `apiauth` untuk proteksi endpoint write (POST/PUT/DELETE)
- ✅ CRUD Artikel via API (`/post`)
- ✅ Login API (`/api/login`) yang menghasilkan token
- ✅ Panel Admin MVC dengan filter session-based auth
- ✅ Database MySQL dengan tabel `artikel`, `kategori`, dan `user`

---

## 🛠️ Teknologi yang Digunakan

| Teknologi | Versi | Fungsi |
|---|---|---|
| **CodeIgniter 4** | `4.x` | Framework PHP MVC utama |
| **PHP** | `>= 7.4` | Bahasa pemrograman backend |
| **MySQL** | `5.7 / 8.x` | Database relasional |
| **XAMPP** | Latest | Web server lokal (Apache + MySQL + PHP) |
| **Composer** | Latest | Package manager PHP |
| **RESTful API** | – | Arsitektur komunikasi API |

---

## 📁 Struktur Direktori

```
lab11_ci4/
└── ci4/
    ├── .env                        # Konfigurasi environment (database, dll)
    ├── composer.json               # Dependensi PHP via Composer
    ├── spark                       # CLI CodeIgniter (php spark)
    ├── public/                     # Document root web server
    │   └── index.php
    ├── app/
    │   ├── Config/
    │   │   ├── Routes.php          # Definisi semua rute URL
    │   │   ├── Filters.php         # Registrasi filter (auth, apiauth)
    │   │   └── Database.php        # Konfigurasi koneksi database
    │   ├── Controllers/
    │   │   ├── Home.php            # Controller halaman utama
    │   │   ├── Artikel.php         # Controller CRUD Artikel (MVC)
    │   │   ├── Post.php            # Controller REST API Artikel (/post)
    │   │   ├── AjaxController.php  # Controller Ajax (praktikum sebelumnya)
    │   │   ├── UserController.php  # Controller login session-based
    │   │   ├── User.php            # Controller manajemen user
    │   │   └── Api/
    │   │       └── Auth.php        # Controller API Login (/api/login)
    │   ├── Models/
    │   │   ├── ArtikelModel.php    # Model tabel artikel + join kategori
    │   │   ├── KategoriModel.php   # Model tabel kategori
    │   │   └── UserModel.php       # Model tabel user
    │   ├── Filters/
    │   │   ├── Auth.php            # Filter autentikasi session (admin panel)
    │   │   └── ApiAuthFilter.php   # Filter token Bearer (proteksi API)
    │   └── Views/
    │       ├── home.php            # View halaman utama
    │       ├── artikel/            # Views CRUD Artikel (admin panel)
    │       ├── layout/             # Layout template
    │       └── user/               # Views login/register user
    ├── system/                     # Core framework CI4 (jangan diubah)
    ├── vendor/                     # Dependensi Composer
    └── writable/                   # Cache, log, session (writable oleh server)
```

---

## ⚙️ Cara Menjalankan

### Prasyarat
- XAMPP (Apache + MySQL) terinstal
- PHP >= 7.4
- Composer terinstal
- Git terinstal

### Langkah 1 – Clone Repository

```bash
git clone https://github.com/<username>/lab11_ci4.git
cd lab11_ci4/ci4
```

### Langkah 2 – Install Dependensi

```bash
composer install
```

### Langkah 3 – Konfigurasi Database

Buat database baru di phpMyAdmin atau MySQL CLI:
```sql
CREATE DATABASE lab_ci4;
```

Buat tabel yang diperlukan:
```sql
-- Tabel Artikel
CREATE TABLE artikel (
    id INT(11) AUTO_INCREMENT PRIMARY KEY,
    judul VARCHAR(200) NOT NULL,
    isi TEXT,
    gambar VARCHAR(200),
    status TINYINT(1) DEFAULT 0,
    slug VARCHAR(200),
    id_kategori INT(11),
    created_at DATETIME
);

-- Tabel Kategori
CREATE TABLE kategori (
    id_kategori INT(11) AUTO_INCREMENT PRIMARY KEY,
    nama_kategori VARCHAR(100) NOT NULL
);

-- Tabel User
CREATE TABLE user (
    id INT(11) AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(100) NOT NULL,
    useremail VARCHAR(100),
    userpassword VARCHAR(255) NOT NULL,
    role VARCHAR(20) DEFAULT 'admin'
);

-- Insert user admin (password: admin123)
INSERT INTO user (username, useremail, userpassword, role)
VALUES ('admin', 'admin@email.com', 'admin123', 'admin');
```

### Langkah 4 – Konfigurasi File `.env`

Edit file `ci4/.env`:
```ini
CI_ENVIRONMENT = development

database.default.hostname = localhost
database.default.database = lab_ci4
database.default.username = root
database.default.password = 
database.default.DBDriver = MySQLi
database.default.port = 3306
```

### Langkah 5 – Jalankan Server

```bash
php spark serve --port 8080
```

Server berjalan di: **http://localhost:8080**

---

## 🔗 Daftar Endpoint REST API

### Auth API

| Method | Endpoint | Deskripsi | Auth |
|---|---|---|---|
| `POST` | `/api/login` | Login dan mendapatkan token | ❌ |
| `OPTIONS` | `/api/login` | CORS Preflight | ❌ |

#### Contoh Request Login:
```json
POST http://localhost:8080/api/login
Content-Type: application/json

{
    "username": "admin",
    "password": "admin123"
}
```

#### Contoh Response Sukses:
```json
{
    "status": 200,
    "error": null,
    "messages": "Login Berhasil",
    "data": {
        "id": 1,
        "username": "admin",
        "token": "VE9LRU4tU0VDUkVULWFkbWlu"
    }
}
```

### Artikel API (/post)

| Method | Endpoint | Deskripsi | Auth |
|---|---|---|---|
| `GET` | `/post` | Tampilkan semua artikel | ❌ |
| `GET` | `/post/{id}` | Tampilkan satu artikel | ❌ |
| `POST` | `/post` | Tambah artikel baru | ✅ Bearer Token |
| `PUT` | `/post/{id}` | Ubah artikel | ✅ Bearer Token |
| `DELETE` | `/post/{id}` | Hapus artikel | ✅ Bearer Token |

#### Contoh Request dengan Token:
```
GET http://localhost:8080/post
Authorization: Bearer VE9LRU4tU0VDUkVULWFkbWlu
```

---

## 🗺️ Alur Kerja API

```
[Frontend Vue.js]
       ↓  POST /api/login
[Api\Auth::login()]
       ↓  cek database user
[UserModel]
       ↓  token digenerate & dikembalikan
[Frontend simpan token di localStorage]
       ↓  setiap request berikutnya: "Authorization: Bearer <token>"
[ApiAuthFilter::before()]
       ↓  token valid → lanjut
[Post Controller (CRUD)]
       ↓  query database
[ArtikelModel]
       ↓  JSON Response
[Frontend Vue.js]
```

---

## 🔐 Mekanisme Keamanan API

### `ApiAuthFilter` (app/Filters/ApiAuthFilter.php)
Filter ini dieksekusi **sebelum** request CRUD masuk ke controller:
1. Membaca header `Authorization` dari request
2. Mengekstrak string token (memisahkan `Bearer ` dari token)
3. Jika token tidak ada atau kosong → respons **401 Unauthorized**
4. Jika token valid → request diteruskan ke controller

### CORS Headers
Setiap controller API mengeset header CORS agar bisa diakses dari frontend yang berbeda origin:
```php
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: GET, POST, OPTIONS, PUT, DELETE');
```

---

## 📸 Halaman Admin (MVC)

Selain REST API, tersedia juga panel admin berbasis MVC:

| URL | Fungsi |
|---|---|
| `http://localhost:8080/` | Halaman utama |
| `http://localhost:8080/artikel` | Daftar artikel publik |
| `http://localhost:8080/admin/artikel` | Panel admin CRUD (perlu login session) |
| `http://localhost:8080/user/login` | Halaman login admin |

---


# Frontend SPA Secured (Vue.js)

> **Praktikum Pemrograman Web Lanjut**

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
👉 **[lab11_ci4 – CodeIgniter 4 REST API](67/67/lab11_ci4)**

---

*Dibuat sebagai bagian dari tugas Praktikum Pemrograman Web – Universitas Pelita Bangsa*
