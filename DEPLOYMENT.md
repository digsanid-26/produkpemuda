# Dokumentasi Deployment Node.js di cPanel

## PT Produk Pemuda Santara - Company Profile Website
**Domain:** http://produkpemuda.com

---

## Informasi Project

| Item | Keterangan |
|------|------------|
| **Framework** | Node.js + Express.js |
| **Template Engine** | EJS |
| **Port Localhost** | 4000 (bukan 3000/3001) |
| **Admin Panel** | /admin |
| **Database** | JSON File-based (folder /data) |

---

## Menu Utama Website

| No | Menu | URL | Keterangan |
|----|------|-----|------------|
| 1 | Home | / | Halaman utama |
| 2 | Profile | /profile | Profil perusahaan |
| 3 | History | /history | Sejarah perusahaan |
| 4 | Produk | /produk | Daftar produk |
| 5 | News & Event | /news | Berita dan event |
| 6 | Contact Us | /contact | Halaman kontak |
| 7 | CSR | /csr | Corporate Social Responsibility |

---

## Struktur Project

```
Finazze/
├── app.js                 # Entry point aplikasi
├── package.json           # Dependencies & scripts
├── config.env             # Konfigurasi environment (JANGAN di-commit)
├── config.env.example     # Template konfigurasi
├── .gitignore             # File yang diabaikan Git
├── data/                  # Database JSON
│   ├── articles.json      # Data artikel
│   ├── portfolio.json     # Data portfolio/produk
│   ├── testimonials.json  # Data testimoni
│   ├── settings.json      # Pengaturan website
│   └── users.json         # Data admin users
├── routes/
│   ├── route.js           # Routes frontend
│   └── admin.js           # Routes admin panel
├── views/
│   ├── admin/             # Views admin panel
│   ├── profile.ejs        # Halaman profile
│   ├── history.ejs        # Halaman history
│   ├── produk.ejs         # Halaman produk
│   ├── news.ejs           # Halaman news & event
│   ├── csr.ejs            # Halaman CSR
│   ├── contact.ejs        # Halaman contact
│   └── ...                # Views lainnya
└── public/                # Static files (CSS, JS, images)
```

---

## Setup di Localhost

### 1. Install Dependencies
```bash
cd Finazze-Nodejs_v1.0/Finazze
npm install
```

### 2. Konfigurasi Environment
```bash
# Copy template config
cp config.env.example config.env

# Edit config.env sesuai kebutuhan
```

**Isi config.env:**
```env
PORT=4000
NODE_ENV=development
SESSION_SECRET=produkpemuda_secret_key_2024
DB_PATH=./data
SITE_NAME=PT Produk Pemuda Santara
SITE_URL=http://localhost:4000
```

### 3. Jalankan Aplikasi
```bash
# Development mode (dengan auto-reload)
npm run dev

# Production mode
npm start
```

### 4. Akses Website
- **Frontend:** http://localhost:4000
- **Admin Panel:** http://localhost:4000/admin
- **Login Admin:** username: `admin` | password: `admin123`

---

## Deployment ke cPanel via Git

### Prasyarat di cPanel
1. **Node.js Selector** harus tersedia di cPanel
2. **Git Version Control** harus aktif
3. **SSH Access** (opsional, untuk kemudahan)

### Langkah 1: Setup Git Repository

```bash
# Di folder project lokal
cd Finazze-Nodejs_v1.0/Finazze

# Inisialisasi Git (jika belum)
git init

# Tambahkan semua file
git add .

# Commit pertama
git commit -m "Initial commit - PT Produk Pemuda Santara website"

# Tambahkan remote repository
git remote add origin https://github.com/USERNAME/produkpemuda.git

# Push ke repository
git push -u origin main
```

### Langkah 2: Setup di cPanel

#### A. Buat Node.js Application
1. Login ke cPanel
2. Cari **"Setup Node.js App"** atau **"Node.js Selector"**
3. Klik **"Create Application"**
4. Isi konfigurasi:
   - **Node.js Version:** 18.x atau 20.x (LTS)
   - **Application Mode:** Production
   - **Application Root:** `produkpemuda` (atau nama folder)
   - **Application URL:** `produkpemuda.com`
   - **Application Startup File:** `app.js`
   - **Passenger Log File:** (biarkan default)

5. Klik **"Create"**

#### B. Clone Repository via Git
1. Di cPanel, cari **"Git Version Control"**
2. Klik **"Create"**
3. Isi:
   - **Clone URL:** `https://github.com/USERNAME/produkpemuda.git`
   - **Repository Path:** `/home/username/produkpemuda`
   - **Repository Name:** `produkpemuda`
4. Klik **"Create"**

#### C. Konfigurasi Environment di Server
1. Masuk ke **File Manager** cPanel
2. Navigasi ke folder aplikasi
3. Buat file `config.env` dengan isi:

```env
PORT=4000
NODE_ENV=production
SESSION_SECRET=GANTI_DENGAN_SECRET_KEY_YANG_AMAN
DB_PATH=./data
SITE_NAME=PT Produk Pemuda Santara
SITE_URL=http://produkpemuda.com
```

#### D. Install Dependencies & Start
1. Kembali ke **Node.js Selector**
2. Klik aplikasi yang sudah dibuat
3. Klik **"Run NPM Install"**
4. Setelah selesai, klik **"Restart"**

---

## Konfigurasi Port di cPanel

### Penting: Port di cPanel
Di cPanel dengan Passenger, **port tidak perlu dikonfigurasi manual**. Passenger akan otomatis menangani routing dari domain ke aplikasi Node.js.

Namun, jika menggunakan **port custom** (bukan 3000/3001):

```env
# Di config.env
PORT=4000
```

Aplikasi ini sudah dikonfigurasi menggunakan **PORT 4000** untuk menghindari konflik dengan aplikasi lain.

### Jika Perlu Port Berbeda
Edit file `config.env`:
```env
PORT=4002  # atau port lain yang tersedia
```

---

## Update Website (Git Push)

### Dari Localhost ke Server

```bash
# 1. Lakukan perubahan di localhost
# 2. Commit perubahan
git add .
git commit -m "Deskripsi perubahan"

# 3. Push ke repository
git push origin main
```

### Di cPanel (Pull Update)
1. Buka **Git Version Control**
2. Klik repository `produkpemuda`
3. Klik **"Update from Remote"** atau **"Pull"**
4. Buka **Node.js Selector**
5. Klik **"Restart"** pada aplikasi

### Atau via SSH (Lebih Cepat)
```bash
ssh username@produkpemuda.com
cd ~/produkpemuda
git pull origin main
# Restart via cPanel atau touch tmp/restart.txt
```

---

## Fitur Admin Panel

### Akses Admin
- **URL:** http://produkpemuda.com/admin
- **Default Login:** 
  - Username: `admin`
  - Password: `admin123`

### Menu Admin
1. **Dashboard** - Statistik konten
2. **Artikel** - CRUD artikel/blog
3. **Portfolio** - CRUD produk/portfolio
4. **Testimoni** - CRUD testimoni pelanggan
5. **Pengaturan** - Konfigurasi website

### Ganti Password Admin
Edit file `data/users.json`:
```json
[
  {
    "id": 1,
    "username": "admin",
    "password": "password_baru_anda",
    "name": "Administrator",
    "email": "admin@produkpemuda.com",
    "role": "admin"
  }
]
```

---

## Troubleshooting

### Error: Port Already in Use
```bash
# Cek port yang digunakan
netstat -tulpn | grep 4000

# Ganti port di config.env
PORT=4002
```

### Error: Cannot Find Module
```bash
# Reinstall dependencies
rm -rf node_modules
npm install
```

### Error: Permission Denied
```bash
# Di cPanel, pastikan permission folder benar
chmod -R 755 /home/username/produkpemuda
```

### Website Tidak Update Setelah Git Pull
1. Clear cache browser
2. Restart aplikasi di Node.js Selector
3. Atau buat file restart:
```bash
touch tmp/restart.txt
```

---

## Informasi Kontak

**PT Produk Pemuda Santara**
- Website: http://produkpemuda.com
- Email: info@produkpemuda.com

---

## Ringkasan Port & URL

| Environment | Port | URL |
|-------------|------|-----|
| Localhost Dev | 4000 | http://localhost:4000 |
| Localhost Admin | 4000 | http://localhost:4000/admin |
| Production | Auto (Passenger) | http://produkpemuda.com |
| Production Admin | Auto | http://produkpemuda.com/admin |

**Catatan:** Port 3000 dan 3001 **tidak digunakan** sesuai permintaan.
