# Aimeos Shop — DigitalSKola Final Project

> **Cloud Full-Stack Deployment** | Laravel 12 + Aimeos eCommerce

[![CI/CD Pipeline](https://github.com/it02srp/DigitalSKola_Final_Project/actions/workflows/ci-cd.yml/badge.svg)](https://github.com/it02srp/DigitalSKola_Final_Project/actions/workflows/ci-cd.yml)

**Live:** https://digitalskolafinalproject-production.up.railway.app
**CI/CD:** https://github.com/it02srp/DigitalSKola_Final_Project/actions
**Repo:** https://github.com/it02srp/DigitalSKola_Final_Project

---

## Deskripsi Proyek

Aplikasi e-commerce berbasis **Laravel 12** dengan package **Aimeos** yang menyediakan fitur toko online lengkap, mulai dari katalog produk, keranjang belanja, hingga proses checkout. Proyek ini dikembangkan sebagai Final Project DigitalSKola bootcamp dengan fokus pada implementasi **CI/CD pipeline** dan praktik **Cloud Deployment**.

---

## Tech Stack

| Layer | Teknologi |
|---|---|
| Backend | PHP 8.2, Laravel 12 |
| eCommerce | Aimeos ~2025.04 |
| Frontend | Blade, Tailwind CSS, Vite |
| Auth | Laravel Breeze |
| Database | MySQL (production), SQLite in-memory (testing) |
| CI/CD | GitHub Actions |
| Code Style | Laravel Pint |
| Testing | PHPUnit 11 |

---

## Fitur

- Katalog produk lengkap dengan filter & pencarian
- Keranjang belanja dan proses checkout
- Manajemen akun pengguna (register, login, profil)
- Admin panel Aimeos untuk manajemen produk & order
- Responsive UI dengan Tailwind CSS

---

## CI/CD Pipeline

Pipeline otomatis berjalan setiap kali ada **push** atau **pull request** ke branch `main`.

```
Push / PR ke main
       │
       ▼
┌─────────────────────┐
│   Build & Test      │  ← PHP 8.2, SQLite in-memory
│  ─────────────────  │
│  composer install   │
│  npm ci && npm build│
│  php artisan test   │
│  pint --test        │
└─────────────────────┘
       │
       ▼ (jika success)
┌─────────────────────┐
│  Deploy to          │
│  Production         │  ← SSH ke cloud server
└─────────────────────┘
```

### Workflow: `.github/workflows/ci-cd.yml`

**Job: Build & Test**
1. Checkout kode
2. Setup PHP 8.2 + extensions (pdo, sqlite, gd, dll)
3. Copy `.env.example` → `.env` & generate app key
4. `composer install` — install PHP dependencies
5. `npm ci && npm run build` — build frontend assets (Vite)
6. `php artisan test` — jalankan test suite dengan SQLite in-memory
7. `./vendor/bin/pint --test` — cek code style

---

## Instalasi Lokal

### Prasyarat

- PHP >= 8.2 (dengan extension: zip, pdo_mysql, gd, intl, bcmath)
- Composer
- Node.js >= 20
- MySQL

### Langkah-langkah

```bash
# 1. Clone repository
git clone https://github.com/it02srp/DigitalSKola_Final_Project.git
cd DigitalSKola_Final_Project

# 2. Install dependencies
composer install
npm install

# 3. Setup environment
cp .env.example .env
php artisan key:generate

# 4. Konfigurasi database di .env
# DB_DATABASE=aimeos_shop
# DB_USERNAME=root
# DB_PASSWORD=

# 5. Migrasi & setup data Aimeos
php artisan migrate
php artisan aimeos:setup

# 6. Build frontend
npm run build

# 7. Jalankan server
php artisan serve
```

Akses di: `http://127.0.0.1:8000`

---

## Menjalankan Tests

```bash
# Jalankan semua test (SQLite in-memory)
php artisan test

# Cek code style
./vendor/bin/pint --test

# Auto-fix code style
./vendor/bin/pint
```

---

## Struktur Direktori

```
shop/
├── .github/workflows/    # CI/CD pipeline
│   └── ci-cd.yml
├── app/                  # Laravel application code
├── config/               # Konfigurasi aplikasi & Aimeos
├── database/             # Migrations & seeders
├── public/               # Entry point & assets build
├── resources/            # Views, CSS, JS
├── routes/               # Route definitions
├── tests/                # Unit & Feature tests
│   ├── Feature/
│   └── Unit/
├── phpunit.xml           # Konfigurasi PHPUnit
└── .env.example          # Template environment
```

---

## Environment Variables

| Variable | Deskripsi | Default (CI) |
|---|---|---|
| `APP_ENV` | Environment aplikasi | `testing` |
| `DB_CONNECTION` | Driver database | `sqlite` (test), `mysql` (prod) |
| `DB_DATABASE` | Nama database | `:memory:` (test) |
| `QUEUE_CONNECTION` | Queue driver | `sync` |
| `CACHE_STORE` | Cache driver | `array` |
| `MAIL_MAILER` | Mail driver | `array` |

---

## Akses Aplikasi

| | URL |
|---|---|
| Shop | https://digitalskolafinalproject-production.up.railway.app |
| Login | https://digitalskolafinalproject-production.up.railway.app/login |
| Register | https://digitalskolafinalproject-production.up.railway.app/register |
| Admin Panel | https://digitalskolafinalproject-production.up.railway.app/admin |

### Cara buat akun admin

1. Register user biasa di `/register`
2. Jalankan perintah berikut di Railway (service → Run Command):
```bash
php artisan aimeos:account --admin emailkamu@example.com
```

---

## Deployment (Railway)

Aplikasi di-deploy ke **Railway.app** menggunakan Nixpacks builder.

| Komponen | Detail |
|---|---|
| Platform | Railway.app |
| Builder | Nixpacks (PHP 8.2 + Node 20) |
| Database | MySQL 8.0 (Railway managed) |
| HTTPS | Otomatis (Railway proxy) |
| Region | SFO (San Francisco) |

Startup sequence saat deploy:
```
config:cache → route:cache → view:cache → migrate --force → aimeos:setup → serve
```

---

## Lisensi

MIT
