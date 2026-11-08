Langkah 1: Kamu (pengirim) siapkan project sebelum di-zip

  1. Pastikan ada file .env.example di root project. Jika belum, buat dari .env tapi hapus nilai
     rahasia (ganti dengan placeholder/contoh).
  2. Pastikan .env tidak ikut di-zip (biasanya sudah di .gitignore). Jika ada file .env fisik di
     folder, hapus atau buang dari zip.
  3. Buang juga folder yang besar dan bisa di-generate ulang dari zip:
     - vendor/
     - node_modules/
     - storage/ (kecuali jika ada file upload penting yang ingin kamu ikutkan)
     - bootstrap/cache/
  4. Zip sisa folder project dan kirim ke penerima.

  Langkah 2: Penerima buka zip di Windows (atau OS lain)

  1. Extract zip ke folder, misal C:\xampp\htdocs\nexstore.
  2. Buka XAMPP Control Panel, nyalakan Apache dan MySQL.
  3. Buka phpMyAdmin (http://localhost/phpmyadmin), buat database baru, misal nama: nexstore.

  Langkah 3: Penerima atur .env

  1. Di folder project, salin .env.example → .env:
  copy .env.example .env
     (atau salin manual via Windows Explorer)
  2. Buka file .env, ubah bagian database agar sesuai komputernya:
  DB_CONNECTION=mysql
  DB_HOST=127.0.0.1
  DB_PORT=3306
  DB_DATABASE=nexstore
  DB_USERNAME=root
  DB_PASSWORD=
     (XAMPP default username root, password kosong)
  3. Simpan file.

  Langkah 4: Penerima install dependensi & kunci aplikasi

  Buka terminal (CMD / PowerShell / Git Bash) di folder project:

  composer install
  php artisan key:generate

  - composer install akan unduh paket PHP yang dibutuhkan (karena vendor tidak di-zip).
  - php artisan key:generate membuat APP_KEY baru di .env (penting, kalau tidak ada akan error
    enkripsi session/cookie).

  Kalau pakai frontend (Vite/Node):
  npm install
  npm run build

  Langkah 5: Jalankan migrasi & seeding

  php artisan migrate
  php artisan db:seed   # opsional, kalau ada data awal yang perlu diisi

  Langkah 6: Jalankan aplikasi

  php artisan serve

  Buka browser: http://127.0.0.1:8000

  ---

  Ringkasan file yang jadi kuncinya

  ┌──────────────────────────────────────┬─────────┬─────────────────────────────────────────┐
  │                 File                 │ Di-zip? │               Keterangan                │
  ├──────────────────────────────────────┼─────────┼─────────────────────────────────────────┤
  │ .env                                 │ ❌      │ Setiap orang buat sendiri dari          │
  │                                      │ Tidak   │ .env.example                            │
  ├──────────────────────────────────────┼─────────┼─────────────────────────────────────────┤
  php artisan key:generate

  - composer install akan unduh paket PHP yang dibutuhkan (karena vendor tidak di-zip).
  - php artisan key:generate membuat APP_KEY baru di .env (penting, kalau tidak ada akan error enkripsi session/cookie).

  Kalau pakai frontend (Vite/Node):
  npm install
  npm run build

  Langkah 5: Jalankan migrasi & seeding

  php artisan migrate
  php artisan db:seed   # opsional, kalau ada data awal yang perlu diisi

  Langkah 6: Jalankan aplikasi

  php artisan serve

  Buka browser: http://127.0.0.1:8000

  ---

  Ringkasan file yang jadi kuncinya

  ┌─────────────────────────────────────────┬──────────┬─────────────────────────────────────────────┐
  │                  File                   │ Di-zip?  │                 Keterangan                  │
  ├─────────────────────────────────────────┼──────────┼─────────────────────────────────────────────┤
  │ .env                                    │ ❌ Tidak │ Setiap orang buat sendiri dari .env.example │
  ├─────────────────────────────────────────┼──────────┼─────────────────────────────────────────────┤
  │ .env.example                            │ ✅ Iya   │ Template yang dikirim ke penerima           │
  ├─────────────────────────────────────────┼──────────┼─────────────────────────────────────────────┤
  │ vendor/                                 │ ❌ Tidak │ Penerima jalankan composer install          │
  ├─────────────────────────────────────────┼──────────┼─────────────────────────────────────────────┤
  │ node_modules/                           │ ❌ Tidak │ Penerima jalankan npm install               │
  ├─────────────────────────────────────────┼──────────┼─────────────────────────────────────────────┤
  │ Kode sumber (app, routes, config, dll.) │ ✅ Iya   │ Ini yang dikirim                            │
  └─────────────────────────────────────────┴──────────┴─────────────────────────────────────────────┘

  Jadi intinya: jangan kirim .env, kirim .env.example, dan penerima menyesuaikan DB_* sendiri di komputernya. Setelah itu jalankan composer install, key:generate, dan migrate.

✻ Churned for 56s
