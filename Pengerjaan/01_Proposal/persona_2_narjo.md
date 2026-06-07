# User Persona 2: The Data-Rich Underserved (UMKM Menengah Modern)

---

## Profil Dasar

* **Nama:** Narjo
* **Usia:** 29 Tahun
* **Pekerjaan:** Pemilik Rolun Coffee, Bojongsoang
* **Merek Kendaraan Pribadi:** Daihatsu Gran Max (Digunakan untuk operasional kafe dan mengangkut stok logistik biji kopi)

---

## Masalah yang Dihadapi (Sudut Pandang Orang Pertama)

> *"Saya sudah menggunakan aplikasi kasir digital (POS) untuk mencatat semua penjualan kopi di kafe saya. Data penjualannya lengkap sekali, ada ribuan baris data transaksi harian terstruktur yang tersimpan di server aplikasi itu. Tapi data itu cuma jadi pajangan pasif saja.*
>
> *Waktu saya berniat mengajukan pinjaman modal usaha ke bank untuk membuka cabang kedua di area kampus sebelah, bank tetap saja mewajibkan saya menyerahkan tumpukan dokumen cetak fisik, rekening koran 3 bulan terakhir, dan proses survei auditnya memakan waktu berminggu-minggu. Saya merasa data transaksi berharga yang saya kumpulkan setiap hari terisolasi dan sia-sia, padahal data tersebut seharusnya bisa langsung membuktikan kalau bisnis kafe saya ini sangat sehat dan layak mendapatkan modal."*

---

## Karakteristik Persona

### Needs (Kebutuhan Utama)

1. **Konversi Aset Data:** Mengintegrasikan data transaksi historis dari aplikasi kasir POS menjadi nilai kelayakan kredit (*credit rating*) secara otomatis.
2. **Proses Pencairan Cepat:** Skema pengajuan dan persetujuan pinjaman ekspansi modal usaha yang instan guna menangkap momentum bisnis yang dinamis.
3. **Prediksi Harga Bahan Baku:** Peringatan dini terkait fluktuasi harga komoditas pangan untuk melakukan *smart procurement* (pengadaan bahan baku kopi/susu secara cerdas).

### Pain Points (Kendala Utama)

* **Data Transaksi Terisolasi:** Data operasional dan penjualan bernilai tinggi tersimpan pasif di vendor aplikasi POS pihak ketiga tanpa memiliki nilai guna finansial.
* **Prosedur Kredit Manual:** Penilaian kelayakan kredit oleh bank konvensional masih mengandalkan dokumen fisik dan verifikasi manual yang lambat.
* **Fluktuasi Harga Bahan Baku:** Rentan terhadap perubahan harga bahan pokok harian yang tidak terprediksi sehingga mengacaukan anggaran operasional bulanan.

---

## Objektif Solusi (AstraPay Flow)

1. **AstraPay Flow-SDK:** Mengintegrasikan modul SDK ringan ke sistem POS kasir yang sudah digunakan Rolun Coffee tanpa perlu mengganti aplikasi kasir tersebut.
2. **Alternatif Credit Scoring:** Mengubah riwayat transaksi POS menjadi skor kredit alternatif legal yang terhubung ke sistem biro kredit untuk persetujuan instan.
3. **Simulasi Pembiayaan Produktif (FINATRA):**
   * **Produk:** FIFGROUP FINATRA (Kredit Usaha Mikro dengan Jaminan BPKB Mobil Daihatsu Gran Max).
   * **Plafon Pinjaman Pokok ($P$):** Rp50.000.000 (Tenor 12 bulan, Bunga flat 14%/tahun atau setara 1,17%/bulan).
   * **Mekanisme Pelunasan:** Pembayaran angsuran harian otomatis disisihkan dari hasil harian QRIS (*split-settlement*) sebesar **Rp158.389 per hari** (~3,96% dari omzet harian rata-rata Rp4.000.000), meminimalisir risiko penunggakan bulanan.
