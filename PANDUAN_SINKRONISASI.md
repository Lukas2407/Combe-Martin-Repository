# ☁️ Panduan Sinkronisasi Antar-Perangkat

Fitur ini menghubungkan **semua perangkat** (HP kasir di cafe, laptop/HP Anda di rumah) sehingga:

- Setiap **transaksi kasir langsung muncul** di perangkat Anda dalam hitungan detik — pantau dari mana saja. 📡
- Anda **mengubah harga/menu dari rumah** → layar kasir otomatis ter-update.
- Laporan penjualan di perangkat Anda selalu berisi gabungan semua kasir.

Data tersimpan di **Firebase** (layanan database Google) pada **akun Anda sendiri** — gratis untuk skala cafe (paket gratisnya sanggup menampung puluhan ribu transaksi per bulan).

---

## Langkah 1 — Buat proyek Firebase (sekali saja, ±5 menit)

1. Buka **https://console.firebase.google.com** → login dengan akun Google Anda.
2. Klik **Create a project** (Buat proyek) → beri nama, mis. `cafe-combe-martin` → **Continue**.
3. Saat ditanya Google Analytics: **matikan** (tidak perlu) → **Create project** → tunggu → **Continue**.

## Langkah 2 — Nyalakan database Firestore

1. Di menu kiri: **Build → Firestore Database** → klik **Create database**.
2. Pilih lokasi server: **asia-southeast2 (Jakarta)** → Next.
3. Pilih **Start in production mode** → **Create**.
4. Buka tab **Rules** (di atas), hapus isinya, ganti dengan ini, lalu klik **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /stores/{storeId}/{document=**} {
      allow read, write: if true;
    }
  }
}
```

> 🔐 **Keamanannya dari mana?** Data Anda tersimpan di bawah **ID Toko** — kode acak panjang yang
> hanya diketahui perangkat Anda (seperti nomor rekening rahasia). Tanpa ID itu, data tidak bisa
> ditemukan. Karena itu: **jangan bagikan ID Toko** ke siapa pun selain perangkat cafe Anda.

## Langkah 3 — Ambil 2 kode proyek

1. Klik ikon **⚙️ (gear) → Project settings** (kiri atas).
2. Gulir ke bawah ke bagian **Your apps** → klik ikon **`</>`** (Web).
3. Beri nama bebas (mis. `kasir`) → **Register app**.
4. Akan tampil kode berisi `firebaseConfig`. Catat **dua** nilai ini saja:
   - `apiKey` — contoh: `AIzaSyB1234…`
   - `projectId` — contoh: `cafe-combe-martin`

## Langkah 4 — Sambungkan aplikasi kasir

Di **perangkat Anda (pemilik)** dulu:

1. Buka aplikasi → **Masuk Admin** → **⚙️ Pengaturan** → kartu **☁️ Sinkronisasi Antar-Perangkat**.
2. Centang **Aktifkan sinkronisasi cloud**.
3. Isi **API Key** dan **Project ID** dari Langkah 3.
4. Klik tombol **🎲** untuk membuat **ID Toko** acak → **catat/salin ID ini**.
5. Klik **💾 Simpan & Sambungkan** → lencana ☁️ di pojok aplikasi berubah menjadi **"Tersinkron"** hijau.
   Data lokal perangkat ini otomatis terunggah ke cloud.

Lalu di **setiap perangkat kasir**:

1. Masuk Admin → Pengaturan → Sinkronisasi.
2. Isi **API Key, Project ID, dan ID Toko yang SAMA PERSIS** → Simpan & Sambungkan.
3. Perangkat kasir otomatis mengikuti data cloud (menu, harga, overhead — semuanya sama).

## Langkah 5 — Uji

1. Di HP kasir: buat satu transaksi (Simpan & Cetak).
2. Di perangkat Anda: buka **📈 Laporan Penjualan** → transaksi tadi muncul dalam ±2 detik,
   disertai notifikasi *"🔄 Data diperbarui dari perangkat lain"*. Selesai! 🎉

---

## Cara kerja & hal yang perlu diketahui

| Hal | Penjelasan |
|---|---|
| Butuh internet? | Ya, untuk sinkron. Tanpa internet aplikasi **tetap jalan** (transaksi tersimpan lokal) dan otomatis menyusul terkirim saat online kembali. |
| Apa saja yang tersinkron | Bahan baku, menu & harga, overhead, profil toko, PIN Admin, dan seluruh invoice. |
| Nomor invoice | Tiap perangkat punya huruf unik (mis. `INV-20260709-A001` vs `-B001`) agar tidak kembar. |
| Menghapus invoice | Terhapus di semua perangkat. |
| "Hapus SEMUA Data" | Juga membersihkan invoice di cloud — hati-hati. |
| Ganti PIN | Berlaku otomatis di semua perangkat. |
| Biaya | Paket gratis Firebase: 1 GB penyimpanan, 50.000 baca & 20.000 tulis per hari — jauh di atas kebutuhan cafe. |

## Pemecahan masalah

- **Lencana ☁️ "Gagal"** → periksa API Key/Project ID (salah ketik adalah penyebab #1), pastikan Firestore sudah dibuat (Langkah 2), dan rules sudah di-Publish.
- **Data tidak muncul di perangkat lain** → pastikan **ID Toko sama persis** di semua perangkat (huruf besar/kecil berpengaruh).
- **Ingin memutus satu perangkat** → hilangkan centang *Aktifkan sinkronisasi* di perangkat itu.
- **ID Toko bocor?** → buat ID Toko baru dengan 🎲 di perangkat pemilik, Simpan & Sambungkan (data terunggah ulang ke "brankas" baru), lalu perbarui ID di perangkat kasir yang sah.
