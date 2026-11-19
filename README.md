# Simulasi CAT SKD CPNS 2026

Website simulasi SKD statis (1 file `index.html`): 110 soal (TWK 30, TIU 35, TKP 45), timer 100 menit, skor otomatis ala BKN, dan pembahasan.

## Isi folder
| File | Fungsi |
|---|---|
| `index.html` | Aplikasi ujian (soal & kunci tersimpan ter-encode di dalamnya) |
| `_headers` | Header keamanan untuk Cloudflare Pages / Netlify (CSP, anti-iframe, HSTS, dll.) |
| `robots.txt` | Minta mesin pencari tidak mengindeks situs |

## Opsi A — GitHub Pages (paling cepat, gratis)
1. Buat akun di github.com → **New repository** → nama misal `simulasi-skd` → pilih **Public** → Create.
2. Klik **Add file → Upload files** → seret `index.html`, `_headers`, `robots.txt` → **Commit changes**.
3. Buka **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main` / `(root)` → Save.
4. Tunggu 1–2 menit. Link: `https://USERNAME.github.io/simulasi-skd/`
5. Centang **Enforce HTTPS** di halaman Settings → Pages.

Catatan: GitHub Pages gratis wajib repo publik dan tidak membaca `_headers`. CSP tetap aktif lewat tag `<meta>` di `index.html`.

## Opsi B — Cloudflare Pages + Cloudflare Access (paling aman, gratis)
Cocok kalau ingin hanya teman tertentu yang bisa membuka.
1. Daftar di dash.cloudflare.com → **Workers & Pages → Create → Pages → Upload assets**.
2. Beri nama proyek → upload folder ini (berisi `index.html`, `_headers`, `robots.txt`) → Deploy. Link: `https://NAMA.pages.dev`.
3. Kunci akses: di proyek Pages buka **Settings → General → Access policy → Enable** (atau lewat **Zero Trust → Access → Applications → Add → Self-hosted**, domain `NAMA.pages.dev`).
4. Buat policy **Allow** → Include → **Emails** → isi email teman-teman (paket gratis Zero Trust: hingga 50 pengguna).
5. Teman membuka link → memasukkan email → menerima kode OTP → baru bisa mengakses.

`_headers` otomatis dibaca Cloudflare Pages (juga Netlify) untuk header keamanan.

## Catatan keamanan (penting dibaca)
- Ini situs statis: semua soal ikut terunduh ke browser pengguna. Kunci jawaban sudah di-encode (tidak terbaca di "View Source" atau Ctrl+F), tetapi orang yang paham JavaScript tetap bisa membongkarnya. Untuk latihan dengan teman, ini cukup. Kalau perlu kunci benar-benar rahasia, penilaian harus dipindah ke server (misalnya Cloudflare Worker).
- Tidak ada data yang dikirim ke server mana pun. Jawaban & progres hanya tersimpan di `localStorage` browser masing-masing peserta.
- Tidak memakai script/font eksternal (CDN), sehingga tidak ada pihak ketiga yang bisa menyisipkan kode.
- Cegah iframe/clickjacking: `frame-ancestors 'none'` + `X-Frame-Options: DENY` (Cloudflare/Netlify) dan skrip anti-frame.
- Jika `index.html` diedit, hash CSP (`sha256-...`) di tag `<meta>` dan di `_headers` harus diperbarui. Kalau tidak, skrip akan diblokir browser.
