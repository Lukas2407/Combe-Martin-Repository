# 📖 Petunjuk Penggunaan — Warung COGS

Aplikasi kalkulator **COGS/HPP (Harga Pokok Penjualan)** + **Kasir & Invoice** untuk usaha restoran/cafe.
Semua rumus mengikuti file Excel `COGS_Final_Combe_Martin.xlsx` Anda.

---

## 1. Cara Mendapatkan & Membuka Aplikasi

Aplikasi hanya terdiri dari **satu file: `index.html`** — tidak perlu instalasi, tidak perlu internet.

1. **Download** file `index.html` dari repositori ini
   (tombol hijau **Code → Download ZIP**, atau buka file `index.html` → tombol **Download raw file**).
2. **Klik dua kali** file tersebut — aplikasi terbuka di browser (Chrome/Edge/Firefox, di laptop maupun HP).
3. Selesai! Saat pertama dibuka, aplikasi otomatis memuat **data demo** dari Excel Anda supaya langsung bisa dicoba.

> 💡 **Tips**: simpan file di Desktop / layar utama HP agar mudah dibuka setiap hari. Di HP Android, buka
> lewat Chrome lalu pilih menu ⋮ → *Tambahkan ke layar utama* agar tampil seperti aplikasi biasa.

### Dipakai beberapa orang di cafe

- Salin file `index.html` ke **setiap perangkat** (kasir 1, kasir 2, laptop pemilik) — kirim via WhatsApp/email/flashdisk.
- **Data tersimpan di masing-masing perangkat** (browser-nya), bukan di file HTML. Menutup browser tidak menghapus data.
- Untuk menyamakan data antar perangkat: di perangkat utama, **Pengaturan → Ekspor Semua Data (JSON)**,
  lalu di perangkat lain **Pengaturan → Impor Data dari File**.
- Kapasitas penyimpanan menggunakan IndexedDB browser — cukup untuk **ratusan ribu transaksi**.

---

## 2. Dua Mode: Kasir & Admin 🔐

| | 👤 **Mode Kasir** (bawaan) | ⭐ **Mode Admin** (butuh PIN) |
|---|---|---|
| Membuat invoice / transaksi | ✅ | ✅ |
| Melihat harga jual menu | ✅ | ✅ |
| Memberi diskon | ⚠️ dibatasi (diatur Admin, bawaan 0%) | ✅ bebas |
| Mengubah harga jual, resep, bahan | ❌ | ✅ |
| Mengatur overhead (gaji, listrik, dll.) | ❌ | ✅ |
| Melihat laporan penjualan & laba | ❌ | ✅ |
| Ekspor/impor/hapus data | ❌ | ✅ |

- **PIN Admin bawaan: `1234`** → **WAJIB segera diganti** di *Pengaturan → Keamanan*.
- Klik **🔓 Masuk Admin** (kiri bawah) untuk membuka mode Admin; klik **🔒 Kunci** untuk kembali ke mode Kasir.
- Mode Admin **terkunci otomatis** setelah 5 menit tidak ada aktivitas.
- Kasir tidak bisa mengubah harga di keranjang — harga selalu diambil dari daftar menu yang dikunci Admin.

> ⚠️ **Catatan keamanan yang jujur**: ini aplikasi offline di browser, jadi perlindungan PIN bersifat
> pencegahan operasional (kasir tidak bisa mengubah harga lewat aplikasi). Orang yang sangat paham teknis
> dan memegang perangkat bisa saja mengutak-atik data browser. Untuk keamanan maksimal: pakai perangkat
> terpisah untuk kasir, jangan beri tahu PIN, dan rutin ekspor backup.

---

## 3. Alur Kerja yang Disarankan (Setup Awal)

Masuk sebagai **Admin**, lalu kerjakan berurutan:

### Langkah 1 — 📊 Dashboard & Overhead
Isi biaya bulanan Anda (sama seperti sheet *Overview* di Excel):
- **Total Gaji Bulanan** semua staff dan **Total Produksi Bulanan** (porsi) → aplikasi menghitung *Labor Cost per Porsi* = gaji ÷ porsi.
- **Listrik, Air, Gas, Lainnya** → *Utilities per Porsi* = total utilitas ÷ porsi.
- Semua menu otomatis memakai nilai ini. Ubah sekali, semua COGS ter-update.

### Langkah 2 — 📦 Bahan Baku
Daftarkan semua bahan (sama seperti sheet *Materials*):
- Isi **Harga Beli per Kemasan** dan **Isi Kemasan** (contoh: 1 karung beras Rp 180.000 isi 50 Kg).
- **Harga per Satuan dihitung otomatis** = harga beli ÷ isi (contoh: Rp 3.600/Kg).
- Saat harga pasar naik/turun, cukup **Edit** bahan tersebut — COGS semua resep yang memakainya langsung ikut berubah.

### Langkah 3 — 🍳 Menu, Resep & COGS
Buat menu (gabungan sheet *Recipes* + *Menu_COGS_Summary*):
- **Menu diracik** → susun resep: pilih bahan + qty per porsi. Biaya bahan = qty × harga satuan.
- **Menu beli jadi** (air mineral, kerupuk, dll.) → cukup isi harga modal per unit.
- **Total COGS** = bahan + overhead per porsi (overhead bisa dimatikan per produk).
- Isi **Margin %** → aplikasi menghitung **Harga Saran** = COGS ÷ (1 − margin).
- Tetapkan **Harga Jual** (tombol ⤵ memakai harga saran dibulatkan ke Rp 500) — inilah harga yang dilihat kasir.
- **Laba/Unit** = harga jual − COGS, tampil langsung di tabel.

### Langkah 4 — ⚙️ Pengaturan
- Isi **nama cafe, alamat, telepon** (tampil di struk), pajak default, dan batas diskon kasir.
- **Ganti PIN Admin!**
- Setelah data siap, hapus data demo dengan **Muat Ulang Data Demo** → lalu edit, atau **Hapus SEMUA Data** untuk mulai dari nol.

---

## 4. Operasional Harian — 🧾 Kasir & Invoice

1. Kasir membuka aplikasi (otomatis mode Kasir).
2. **Klik produk** untuk memasukkan ke keranjang; atur jumlah dengan tombol **− / +**. Bisa cari nama atau filter kategori (Makanan/Minuman/Lainnya).
3. Isi nama pelanggan/meja (opsional), diskon (jika diizinkan), pajak.
4. Masukkan **Uang Dibayar** (tombol **Pas** = uang pas) — kembalian dihitung otomatis.
5. Klik **💾 Simpan & Cetak** → invoice tersimpan permanen dengan nomor otomatis (`INV-YYYYMMDD-001`) dan struk siap dicetak.
6. Struk transaksi hari ini bisa **dicetak ulang** dari daftar *Transaksi Terakhir Hari Ini*.

> 🖨️ Mencetak memakai dialog print browser — bisa ke printer kasir (thermal), printer biasa, atau *Save as PDF* untuk dikirim ke pelanggan via WhatsApp.

---

## 5. 📈 Laporan Penjualan (Admin)

- Filter berdasarkan rentang tanggal.
- Ringkasan otomatis: **jumlah transaksi, omzet, HPP terjual, laba kotor** (+% margin nyata).
- Klik 👁️ untuk melihat/cetak ulang invoice; 🗑️ untuk menghapus (Admin saja).
- **⬇️ Ekspor CSV** → buka di Excel untuk analisis lanjutan (pemisah titik-koma, siap untuk Excel Indonesia).

---

## 6. 💾 Backup & Pemindahan Data

| Tindakan | Caranya |
|---|---|
| Backup rutin (disarankan mingguan) | Pengaturan → **Ekspor Semua Data (JSON)** → simpan file di Google Drive/flashdisk |
| Pindah ke perangkat baru | Ekspor di perangkat lama → Impor di perangkat baru |
| Ganti browser/HP | Sama seperti di atas — data tidak otomatis ikut |
| Kembalikan data demo | Pengaturan → **Muat Ulang Data Demo** |

> ⚠️ Jangan menghapus *data situs/site data* browser sebelum ekspor backup — itu akan menghapus data aplikasi.

---

## 7. Rumus yang Dipakai (identik dengan Excel Anda)

| Perhitungan | Rumus |
|---|---|
| Harga per satuan bahan | harga beli ÷ isi kemasan |
| Biaya bahan per porsi | Σ (qty per porsi × harga satuan) |
| Labor cost / porsi | total gaji bulanan ÷ produksi bulanan |
| Utilitas / porsi | (listrik + air + gas + lainnya) ÷ produksi bulanan |
| **Total COGS** | bahan + labor + utilitas |
| Harga saran | COGS ÷ (1 − margin%) |
| Laba per unit | harga jual − COGS |
| Laba kotor laporan | omzet − Σ (COGS item × qty terjual) |

---

## 8. Pertanyaan Umum

**Data hilang saat file HTML dipindah/di-copy?**
Tidak — data tersimpan di browser perangkat, bukan di file. Tapi file yang di-copy ke perangkat lain mulai dengan data demo baru; gunakan ekspor/impor untuk memindahkan data.

**Lupa PIN Admin?**
Tidak ada jalan pintas dari dalam aplikasi (memang disengaja demi keamanan). Solusi: impor file backup JSON yang lama (PIN ikut ter-restore), atau hapus data situs browser (semua data hilang, PIN kembali `1234`) lalu impor backup.

**Berapa banyak data yang bisa disimpan?**
IndexedDB browser umumnya mengizinkan ratusan MB — setara ratusan ribu invoice. Aplikasi akan memberi peringatan bila penyimpanan penuh.

**Apakah butuh internet?** Tidak. 100% offline.

**Bisa di HP?** Bisa — tampilan otomatis menyesuaikan layar kecil.
