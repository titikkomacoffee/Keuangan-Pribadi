# Dompetku – Setup

1. **Supabase**: buat project di supabase.com → SQL Editor → jalankan isi `schema.sql`.
2. **Auth**: Authentication → Providers → Email aktif. Untuk langsung bisa login tanpa verifikasi email, matikan "Confirm email". Di Authentication → URL Configuration isi *Site URL* dengan `https://USERNAME.github.io/NAMA-REPO/`.
3. **Kunci**: Project Settings → API → salin *Project URL* dan *anon public key*, tempel ke bagian `SUPABASE_URL` dan `SUPABASE_KEY` di atas `<script>` pada `index.html`. (Anon key memang publik; data diamankan oleh RLS di `schema.sql`. Jangan pakai service_role key.)
4. **GitHub Pages**: buat repo, upload `index.html` (boleh juga README & schema.sql) → Settings → Pages → Branch `main` / root → Save. Tunggu ±1 menit.
5. Daftar akun di web kamu. Ulangi di HP lain dengan akun yang sama; data muncul real-time.

Tiap pengguna punya data sendiri (terpisah oleh RLS).
