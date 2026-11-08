# Kasir UMKM Offline

Aplikasi kasir (POS) ringan untuk warung, kedai kopi, toko kecil, sampai booth bazar. Cukup buka di browser HP, tablet, atau laptop. Semua data tersimpan di perangkat itu sendiri, jadi tetap jalan walau sinyal hilang.

Dibuat murni dengan HTML, CSS, dan JavaScript. Tidak perlu server, database, atau langganan apa pun.

## Fitur

- **Produk & stok**: kategori, stok per produk, dan varian harga (misal Reguler/Large, Hot/Ice).
- **Kasir cepat**: cari produk, filter kategori, ketuk untuk masuk keranjang, atur jumlah.
- **Diskon** dalam Rupiah atau persen.
- **Metode bayar**: tunai (dengan tombol uang cepat + kembalian otomatis), QRIS (gambar statis), transfer bank.
- **Struk 58mm** siap cetak ke printer thermal lewat dialog cetak browser.
- **Riwayat transaksi** per tanggal, cetak ulang, dan batalkan (stok kembali otomatis).
- **Laporan** harian (per jam) dan bulanan (per tanggal), omzet, rata-rata per struk, produk terlaris, ringkasan metode bayar.
- **Ekspor/impor**: produk ke/dari CSV, transaksi ke CSV (bisa dibuka di Excel/Google Sheets), backup & pulihkan semua data dalam JSON.
- **Multi-kasir** dengan PIN 4 angka. Peran *admin* bisa kelola produk/pengaturan; peran *kasir* hanya jualan.
- **PWA**: bisa di-install seperti aplikasi dan dibuka tanpa internet.
- **Data contoh** (kedai kopi + transaksi 30 hari) supaya langsung bisa dicoba.

## Cara menjalankan (pemula)

### Coba di komputer
1. Unduh/clone folder ini.
2. Buka terminal di folder ini lalu jalankan `python3 -m http.server 8000` (atau pakai ekstensi *Live Server* di VS Code).
3. Buka `http://localhost:8000` di Chrome.
4. Pilih **Admin**, masukkan PIN **1234**. Kasir contoh lain: **Sari**, PIN **1111**.

> Bisa juga klik ganda `index.html`, tapi fitur *install* dan offline (service worker) hanya aktif lewat `http://localhost` atau HTTPS.

### Pakai beneran di HP
1. Unggah folder ini ke hosting statis gratis yang HTTPS: GitHub Pages, Netlify, Vercel, atau Cloudflare Pages.
   - GitHub Pages: buat repo, unggah semua berkas, buka **Settings → Pages → Deploy from branch → main / root**.
2. Buka alamatnya di Chrome HP, lalu menu ⋮ → **Install aplikasi** / **Tambahkan ke layar utama**.
3. Setelah sekali dibuka, aplikasi tetap jalan walau mode pesawat.

### Langkah awal setelah masuk
1. Buka **Atur** → isi profil toko (nama, alamat, telepon, catatan struk, nomor rekening).
2. Ganti PIN: tambah kasir/admin baru dengan PIN sendiri, lalu hapus akun contoh.
3. Di **Atur → Data**, klik **Hapus semua data** kalau mau mulai dari nol (PIN admin kembali 1234).
4. Tambah produk di menu **Produk**, atau impor CSV.

### Format CSV produk
```
nama,kategori,stok,varian
Kopi Susu,Kopi,40,Reguler:18000|Large:23000
Roti Bakar,Camilan,20,Cokelat:15000|Keju:17000
```
Produk dengan nama sama akan diperbarui, bukan diduplikasi.

### Mencetak struk 58mm
Sambungkan printer thermal (USB/Bluetooth) ke perangkat, lalu tekan **Cetak struk**. Di dialog cetak pilih printernya, ukuran kertas 58mm, margin *None*. Di Android biasanya perlu aplikasi layanan cetak dari pembuat printer (misal RawBT).

## Struktur folder
```
kasir-umkm/
├── index.html            # kerangka halaman
├── style.css             # tampilan + gaya struk cetak
├── app.js                # semua logika aplikasi
├── tema.css, tema.js     # warna, sudut, huruf, nama usaha, mode
├── DESIGN.md             # arah desain, token, komponen, motion
├── AGENTS.md, .claude/   # panduan untuk AI (Claude, Cursor, Copilot)
├── sw.js                 # service worker (offline)
├── manifest.webmanifest  # info PWA
├── ikon.svg              # ikon aplikasi
├── qris.svg              # CONTOH QRIS — ganti dengan QRIS tokomu
├── meta.json, LICENSE, README.md, ss.png
```

## Ganti warna & nama
Semua warna, sudut, huruf, dan nama usaha diatur di dua berkas kecil (juga yang diubah otomatis oleh customizer di malikisaputra.biz.id/kode):
- `tema.css`: `--tema-utama` (tombol, sorotan), `--tema-aksen` (menu aktif, uang cepat, kembalian, nada kotak ilustrasi), `--tema-radius` (sudut), `--tema-font` (huruf Google Fonts: Plus Jakarta Sans, Inter, Poppins, DM Sans, Space Grotesk, Nunito — ganti juga `href` pada `<link id="fontTema">` di `index.html`).
- `tema.js`: `nama` (nama usaha di layar; dipakai sebagai nama toko bawaan selama belum diganti lewat menu **Atur**) dan `mode`: `"terang"`, `"gelap"`, atau `"auto"` (ikut setelan HP).

Warna lain (versi muda/tua, latar, garis, mode gelap) diturunkan otomatis dengan `color-mix()` di `style.css`. Teks di atas warna utama dihitung otomatis (putih/gelap) dari terang-gelapnya warna, tapi warna utama yang cukup gelap (setara teal-700 / ungu-600) paling enak dibaca. Arah desain lengkap ada di `DESIGN.md`.

Warna per bagian: `tema.css` juga punya `--tema-latar` (latar halaman), `--tema-panel` (kartu/panel/form), `--tema-nav` (sidebar/header/nav bawah), `--tema-teks` (teks utama). Mode gelap memakai palet gelap paket kecuali customizer mengirim nilai bagian. Warna teks di atas nav & tombol utama dihitung otomatis (terang/gelap) dari luminansi.

## Ubah brand lainnya
- Nama toko di struk: menu **Atur** di aplikasi (setelah diganti di sini, `tema.js` tidak lagi menimpanya).
- QRIS: simpan gambar QRIS tokomu (PNG/SVG), beri nama `qris.svg` (atau ubah `src` di `app.js`, cari `qris.svg`).
- Ikon: ganti `ikon.svg`. Kalau berkas diubah, naikkan versi `CACHE` di `sw.js` (misal `kasir-umkm-v2`) agar HP mengambil versi baru.

## Ubah pakai Claude
Paket ini membawa Agent Skill di `.claude/skills/ubah-kasir-umkm/SKILL.md` (peta berkas, letak tema, cara menambah fitur, aturan aman) dan `AGENTS.md` untuk Cursor/Copilot/Codex.

- **Claude Code / Claude Desktop**: buka folder paket ini; skill di `.claude/skills` terbaca otomatis. Tinggal minta, misal "tambah field barcode di produk".
- **claude.ai**: unduh ZIP skill (tombol **Unduh Skill** di [malikisaputra.biz.id/kode](https://malikisaputra.biz.id/kode)), lalu di claude.ai buka **Customize › Skills › tombol "+" › Create skill › Upload a skill** dan pilih ZIP itu (folder skill sebagai akar ZIP). Syarat: **Code execution and file creation** aktif di **Settings › Capabilities** (paket Free/Pro/Max); di Team/Enterprise diatur oleh Owner. Sumber: support.claude.com/en/articles/12512180 (dicek 6 Nov 2026).

## Mau hasil lebih maksimal?
Pasang skill desain berikut di Claude Code sebelum minta perubahan tampilan:
- impeccable — https://github.com/pbakaus/impeccable (Apache-2.0, dicek 6 Nov 2026)
- ui-ux-pro-max — https://github.com/nextlevelbuilder/ui-ux-pro-max-skill (MIT menurut GitHub API, dicek 6 Nov 2026)

## Catatan penting
- Data tersimpan di browser perangkat (localStorage). Menghapus data situs/browser = data hilang. **Rajin backup JSON** (misal tiap minggu) dan simpan di Google Drive.
- PIN di sini untuk membedakan kasir, bukan pengamanan tingkat bank.
- QRIS yang disertakan hanya gambar contoh, bukan kode pembayaran asli.

## Lisensi
MIT — bebas dipakai, diubah, dan dijual ulang, asal mencantumkan berkas LICENSE.

---
Dibuat oleh **Maliki Studio** — [malikisaputra.biz.id](https://malikisaputra.biz.id), follow TikTok/IG **@malikisaputra**

---

<!-- kredit-maliki -->
**Kredit:** Template oleh Maliki Saputra (@malikisaputra) — https://malikisaputra.biz.id. Kredit di footer & kepala berkas wajib dipertahankan (lihat LICENSE).
