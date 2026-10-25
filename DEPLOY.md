# Panduan Deploy Dompetku (dari nol sampai online)

Total waktu ±30 menit. Semua gratis. Kamu butuh: email, akun GitHub, akun Supabase.

## Paket yang tersedia
| Paket | Harga | Cara kerja |
|---|---|---|
| Demo 1 bulan | Gratis | Otomatis untuk setiap pendaftar baru (30 hari). Atau coba cepat lewat akun demo **Admin / Admin123** |
| 1 Bulan | Rp 10.000 | Admin tekan **+1 bln** |
| Hemat 6 Bulan | Rp 50.000 | Admin tekan **+6 bln** |
| Premium 12 Bulan | Rp 100.000 | Admin tekan **+12 bln** |
| Sekali Bayar | Rp 250.000 | Admin tekan **♾ Selamanya** |
| Developer | Gratis selamanya | Akunmu sendiri, dapat menu **Kelola pelanggan** |

> **PENTING – akun "Admin / Admin123":** password ini tertulis di halaman login sehingga diketahui siapa saja. Karena itu akun tersebut dibuat sebagai **akun demo biasa** (tidak bisa mengelola pelanggan, datanya dipakai bersama penguji, jangan isi data pribadi). Akun **developer** harus memakai emailmu sendiri dan password kuat yang dirahasiakan. Jangan pernah menjadikan Admin123 sebagai developer: siapa pun bisa mengaktifkan langganan gratis untuk dirinya.

---
## TAHAP 1 – Supabase (database)
1. Buka https://supabase.com → **Sign in** → **New project**. Isi nama `dompetku`, buat *Database Password* (simpan), pilih region **Southeast Asia (Singapore)** → **Create new project**. Tunggu ±2 menit.
2. Menu kiri **SQL Editor** → **New query**.
   - Buka file `schema.sql`, salin semua isinya, tempel, klik **Run**. Harus muncul "Success".
   - Query baru lagi: salin semua `schema_langganan.sql`, tempel, **Run**. Urutannya wajib: `schema.sql` dulu, baru `schema_langganan.sql`.
3. **Authentication → Sign In / Providers → Email**: pastikan *Enable Email provider* aktif. Matikan **Confirm email** supaya pendaftar bisa langsung masuk (kalau dibiarkan aktif, pendaftar harus klik link di email dulu). Simpan.
4. **Project Settings → API**: salin *Project URL* dan *anon public key* (sudah terpasang di `index.html` untuk project milikmu; jika membuat project baru, ganti dua nilai itu di bagian atas `<script>`).

## TAHAP 2 – Akun demo (Admin / Admin123)
1. **Authentication → Users → Add user → Create new user**.
   - Email: `admin@dompetku.app`  · Password: `Admin123`
   - Centang **Auto Confirm User** → **Create user**.
2. SQL Editor, jalankan:
```sql
update subscriptions set role='demo' where email='admin@dompetku.app';
```
Di halaman login, pengguna cukup mengetik username `Admin` (aplikasi otomatis mengubahnya jadi admin@dompetku.app) atau menekan tombol **Coba demo**.

## TAHAP 3 – Isi pengaturan di index.html
Buka `index.html` dengan Notepad/VS Code, cari baris `const WA=` (di bagian atas `<script>`):
- `WA='6281234567890'` → ganti dengan nomor WhatsApp-mu, format 62 tanpa 0 dan tanpa +.
- `PAY_INFO='...'` → tulis cara bayar, contoh: `Transfer BCA 1234567890 a.n. Nama Kamu, atau QRIS (kirim bukti bayar)`.
Simpan file.

## TAHAP 4 – GitHub Pages
1. https://github.com → **New repository**. Nama mis. `dompetku`, **Public**, centang *Add a README* → **Create repository**.
2. **Add file → Upload files**, seret file-file ini (isi folder, bukan foldernya):
   `index.html`, `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`, `icon-maskable.png`, `apple-touch-icon.png` → **Commit changes**.
   (File `.sql` dan `.md` boleh tidak diupload.)
3. **Settings → Pages** → *Build and deployment*: Source **Deploy from a branch**, Branch **main** / folder **/(root)** → **Save**.
4. Tunggu 1–2 menit, refresh halaman Pages sampai muncul alamat `https://USERNAME.github.io/dompetku/`.

## TAHAP 5 – Hubungkan alamat ke Supabase
**Authentication → URL Configuration**: isi **Site URL** dengan alamat Pages di atas (pakai `https://` dan garis miring akhir) → **Save**.

## TAHAP 6 – Jadikan dirimu Developer (gratis selamanya)
1. Buka alamatmu → **Daftar** dengan emailmu sendiri dan password kuat.
2. SQL Editor:
```sql
update subscriptions set role='developer' where email='EMAILMU@contoh.com';
```
3. Keluar lalu masuk lagi. Di **Lainnya** muncul **Kelola pelanggan** dan statusmu "Developer – gratis selamanya".

## TAHAP 7 – Uji coba (checklist)
- [ ] Daftar akun baru (email lain) → masuk, di Beranda muncul "Demo gratis: 30 hari lagi".
- [ ] Tombol **Coba demo** → masuk sebagai Admin, tidak ada menu Kelola pelanggan.
- [ ] Catat transaksi di HP dan di komputer dengan akun sama → muncul real-time di keduanya.
- [ ] Simulasi habis demo: SQL `update subscriptions set trial_ends_at=now()-interval '1 day' where email='EMAIL_UJI';` → akun uji melihat halaman paket. Daftar paket → WhatsApp.
- [ ] Di Kelola pelanggan tekan **+1 bln** untuk akun uji → halaman pembayaran di HP-nya terbuka sendiri.
- [ ] **Instal**: Android Chrome → Lainnya → Pasang aplikasi; iPhone Safari → Bagikan → Tambah ke Layar Utama.

## Alur harian
Pelanggan memilih paket → chat WhatsApp-mu → bayar → kamu cek mutasi → buka **Lainnya → Kelola pelanggan** → tekan tombol paket yang sesuai (+1 / +6 / +12 bln, atau ♾ Selamanya). ⛔ untuk mencabut akses. Perpanjangan menambah dari tanggal berakhir, jadi sisa hari tidak hilang.

## Memperbarui aplikasi
Edit file → di GitHub buka file → ikon pensil / **Upload files** menimpa → Commit. Ubah angka `dompetku-v3` di `sw.js` (jadi v4, dst.) agar pengguna menerima versi baru.

## Masalah umum
| Gejala | Solusi |
|---|---|
| "Invalid API key" / layar putih | URL atau anon key di `index.html` salah, atau belum upload versi terbaru |
| Daftar berhasil tapi tidak bisa masuk | **Confirm email** masih aktif (matikan) atau cek email konfirmasi |
| Setelah masuk muncul halaman paket padahal baru daftar | `schema_langganan.sql` belum dijalankan / baris `subscriptions` belum terbentuk. Jalankan ulang file itu (aman diulang) |
| Admin / Admin123 "Invalid login" | User demo belum dibuat di Tahap 2 atau lupa centang Auto Confirm |
| Menu Kelola pelanggan tidak ada | Perintah `role='developer'` belum dijalankan, atau belum keluar-masuk |
| Data tidak muncul real-time | Jalankan ulang bagian Realtime di SQL; pastikan koneksi internet |
| Tombol pasang tidak muncul | Harus lewat `https://`, buka di Chrome/Safari (bukan browser dalam aplikasi chat) |

## Keamanan
- Anon key memang publik; yang menjaga data adalah RLS di database. **Jangan** pernah menaruh `service_role` key di `index.html`.
- Penguncian langganan dilakukan di database, jadi tidak bisa dilewati dengan mengubah tampilan di browser.
- Pembayaran masih manual. Pembayaran otomatis (Midtrans, dll.) bisa ditambahkan nanti.
