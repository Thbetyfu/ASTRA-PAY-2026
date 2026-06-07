# BAB 3 — TARGET USER & USE CASE

> *"Solusi teknologi yang gagal memahami siapa penggunanya pada akhirnya hanya menjadi software tanpa pengguna. Segmentasi berbasis perilaku keuangan — bukan demografi — adalah kunci adopsi massal."*

---

## 3.1 Metodologi Segmentasi: Behavior-Driven Financial Segmentation

Penetapan target pengguna AstraPay Flow tidak bertumpu pada asumsi demografis makro semata. Kami menggunakan **Behavior-Driven Financial Segmentation** — sebuah pendekatan yang mengklasifikasikan pelaku usaha berdasarkan pola perilaku keuangan aktual, hambatan psikologis terhadap teknologi, dan kesiapan infrastruktur digital yang mereka miliki.

Pendekatan ini menghasilkan **tiga arketipe pengguna** yang memiliki karakteristik, jalur adopsi, dan proposisi nilai yang berbeda secara fundamental — namun ketiganya dapat dilayani oleh infrastruktur AstraPay Flow yang sama.

---

## 3.2 Tiga Arketipe Pengguna

### Arketipe 1: The Analog Operator — UMKM Mikro Tradisional

**Representasi Persona:** Ibu Siti — Pemilik Warteg Mulya (2 cabang), Bojongsoang.

**Profil Operasional:**
* Omzet harian: Rp600.000 – Rp1.200.000 (sangat dipengaruhi musim libur kuliah).
* Metode pencatatan: Laci kas tunggal (tanpa pemisahan kas bisnis dan pribadi).
* Teknologi yang dimiliki: Smartphone Android entry-level.
* Status digital saat ini: Belum memiliki QRIS resmi, transaksi didominasi uang tunai.
* Literasi keuangan: Rendah — tidak familiar dengan istilah pembukuan laba-rugi, cash flow, atau suku bunga perbankan.

**Hambatan Utama:**
Bukan ketidakmauan menggunakan teknologi — melainkan **waktu dan kenyamanan fisik**. Tangan basah atau berminyak saat memasak di warung ramai membuat proses pengetikan menu pada aplikasi kasir digital menjadi beban operasional yang memperlambat transaksi.

**Proposisi Nilai Spesifik:**
* **Astra-Voice Floating Widget:** Tombol melayang untuk pencatatan transaksi via perintah suara tanpa menyentuh layar.
* **Zero-Friction Onboarding:** QRIS merchant aktif dalam 5 menit melalui bantuan agen surveyor lapangan FIFGROUP.
* **Pemisahan Kas Bisnis-Pribadi Otomatis:** Dana settlement langsung mengalir ke rekening bisnis Bank Saqu.

**Detail Simulasi Pembiayaan (DANASTRA BPKB Motor):**
* **Produk Pembiayaan:** FIFGROUP DANASTRA (Pembiayaan produktif dengan jaminan BPKB Motor).
* **Plafon Pinjaman Pokok ($P$):** Rp15.000.000.
* **Tenor:** 6 bulan.
* **Suku Bunga flat (DANASTRA):** **2,5% per bulan** (flat).
* **Komponen Biaya Potongan Awal (Upfront Cost Resmi OJK RIPLAY):**
  * Biaya Administrasi: Rp1.710.000
  * Biaya Asuransi: Rp80.000
  * Biaya Jaminan Fidusia: Rp50.000
  * **Total Potongan Biaya:** **Rp1.840.000**
* **Dana Bersih yang Diterima Merchant:** Rp15.000.000 - Rp1.840.000 = **Rp13.160.000**.
* **Total Bunga:** $15.000.000 \times 2,5\% \times 6 = \text{Rp } 2.250.000$.
* **Total Utang:** Rp15.000.000 + Rp2.250.000 = **Rp17.250.000**.
* **Angsuran Bulanan:** Rp17.250.000 / 6 = **Rp2.875.000**.
* **Target Cicilan Harian ($D_{\text{target}}$):** Rp2.875.000 / 30 hari = **Rp95.833 per hari**.
* **Proporsi Cicilan vs Omzet Harian:** Rp95.833 / Rp900.000 (omzet rata-rata) = **10,6%**.

**Journey Adopsi:**
```
Hari 1   : Agen lapangan FIFGROUP datang, OCR KTP, akun Bank Saqu & QRIS aktif.
Minggu 1 : Ibu Siti mulai memakai Voice Widget untuk mencatat transaksi harian.
Bulan 1  : AI mengirim laporan untung-rugi mingguan otomatis via notifikasi suara.
Bulan 3  : Data transaksi QRIS 3 bulan terakumulasi di Astra Financial Data Layer.
Bulan 4  : Pengajuan pinjaman DANASTRA Rp15 juta disetujui instan berbasis data alternatif.
```

---

### Arketipe 2: The Data-Rich Underserved — UMKM Menengah Modern

**Representasi Persona:** Narjo — Pemilik Rolun Coffee, Bojongsoang.

**Profil Operasional:**
* Omzet harian: Rp3.000.000 – Rp6.000.000.
* Metode pencatatan: POS digital (Majoo/Notain), data transaksi harian terstruktur.
* Teknologi yang dimiliki: Tablet kasir, printer struk, QRIS CPM aktif.
* Status digital saat ini: Transaksi digital tinggi, data tersimpan pasif di server POS pihak ketiga.
* Literasi keuangan: Moderat — memahami laporan keuangan dasar, namun tidak tahu cara memanfaatkan datanya untuk mendapat modal resmi.

**Hambatan Utama:**
**Paradoks Informasi.** Memiliki ribuan titik data transaksi bernilai tinggi, tetapi data tersebut terisolasi di server POS vendor kasir independen. Akibatnya, saat mengajukan modal ekspansi bisnis ke bank, Narjo tetap diwajibkan menyerahkan dokumen fisik manual dan melalui proses audit yang memakan waktu berminggu-minggu.

**Proposisi Nilai Spesifik:**
* **AstraPay Flow-SDK:** Injeksi modul SDK ringan ke POS existing tanpa mengubah sistem kasir.
* **Credit Rating Asset Conversion:** Riwayat transaksi POS otomatis diubah menjadi skor kredit alternatif.
* **Predictive Procurement Alert:** Peringatan volatilitas harga bahan baku dari Badan Pangan Nasional terintegrasi Vertex AI.

**Detail Simulasi Pembiayaan (FINATRA Bisnis):**
* **Produk Pembiayaan:** FIFGROUP FINATRA (Kredit Usaha Mikro dengan Jaminan BPKB Mobil/Sertifikat Properti).
* **Plafon Pinjaman Pokok ($P$):** Rp50.000.000.
* **Tenor:** 12 bulan (1 tahun).
* **Suku Bunga flat (FINATRA):** **14% per tahun** (setara **1,17% per bulan**).
* **Komponen Biaya Potongan Awal (Upfront Cost Resmi OJK RIPLAY):**
  * Biaya Administrasi: Rp1.710.000
  * Biaya Asuransi (Jaminan Mobil): Rp350.000
  * Biaya Jaminan Fidusia: Rp50.000
  * **Total Potongan Biaya:** **Rp2.110.000**
* **Dana Bersih yang Diterima Merchant:** Rp50.000.000 - Rp2.110.000 = **Rp47.890.000**.
* **Total Bunga:** $50.000.000 \times 1,17\% \times 12 = \text{Rp } 7.020.000$.
* **Total Utang:** Rp50.000.000 + Rp7.020.000 = **Rp57.020.000**.
* **Angsuran Bulanan:** Rp57.020.000 / 12 = **Rp4.751.667**.
* **Target Cicilan Harian ($D_{\text{target}}$):** Rp4.751.667 / 30 hari = **Rp158.389 per hari**.
* **Proporsi Cicilan vs Omzet Harian:** Rp158.389 / Rp4.000.000 (omzet rata-rata) = **3,96%**.

**Journey Adopsi:**
```
Hari 1   : Integrasi SDK AstraPay Flow ke sistem POS Rolun Coffee (<30 menit).
Minggu 1 : Data transaksi historis 6 bulan terakhir diunggah ke Data Layer Astra.
Bulan 1  : AI mengidentifikasi tren menu terlaris dan memprediksi kebutuhan stok kopi.
Bulan 3  : Skor kredit alternatif (POJK 29/2024) terbentuk di PT PEFINDO Biro Kredit.
Bulan 4  : Pembiayaan FINATRA Rp50 juta disetujui otomatis untuk membuka cabang baru.
```

---

### Arketipe 3: The Platform-Trapped — UMKM Transisi Terkunci Platform

**Representasi Persona:** Pak Yosef — Pemilik Jus BMC, Bojongsoang.

**Profil Operasional:**
* Omzet harian: Rp400.000 – Rp800.000.
* Metode pencatatan: Kasir digital entry-level (GoPay Merchant).
* Status digital saat ini: Memiliki QRIS, namun data transaksi terkunci di ekosistem pesaing.
* Literasi keuangan: Rendah-Moderat — ingin berkembang tetapi terhambat modal.

**Hambatan Utama:**
**Ketiadaan Agunan Fisik (No-Collateral Barrier).** Pak Yosef tidak memiliki aset berharga seperti BPKB kendaraan atau sertifikat tanah. Meskipun transaksinya sudah tercatat secara digital pada platform merchant lain, ia tetap tidak memiliki akses ke pembiayaan produktif konvensional karena syarat jaminan fisik yang kaku.

**Proposisi Nilai Spesifik:**
* **Bank Saqu Saku Kredit:** Pinjaman digital mikro tanpa agunan fisik berbasis reputasi transaksi digital.
* **Ecosystem Loyalty Switching Incentive:** Subsidi nyata berupa token listrik PLN dan servis oli AHASS gratis untuk meringankan biaya switching platform.

**Detail Simulasi Pembiayaan (Bank Saqu Saku Kredit - Tanpa Agunan):**
* **Produk Pembiayaan:** Bank Saqu Saku Kredit (Unsecured Digital Micro Loan).
* **Plafon Pinjaman Pokok ($P$):** Rp20.000.000.
* **Tenor:** 12 bulan (1 tahun).
* **Suku Bunga flat (Bank Saqu):** **1,5% per bulan** (flat).
* **Komponen Biaya Potongan Awal (Upfront Cost Resmi OJK RIPLAY):**
  * Biaya Administrasi/Provisi (1%): Rp200.000
  * Biaya Asuransi Kredit: Rp80.000
  * **Total Potongan Biaya:** **Rp280.000**
* **Dana Bersih yang Diterima Merchant:** Rp20.000.000 - Rp280.000 = **Rp19.720.000**.
* **Total Bunga:** $20.000.000 \times 1,5\% \times 12 = \text{Rp } 3.600.000$.
* **Total Utang:** Rp20.000.000 + Rp3.600.000 = **Rp23.600.000**.
* **Angsuran Bulanan:** Rp23.600.000 / 12 = **Rp1.966.667**.
* **Target Cicilan Harian ($D_{\text{target}}$):** Rp1.966.667 / 30 hari = **Rp65.556 per hari**.
* **Proporsi Cicilan vs Omzet Harian:** Rp65.556 / Rp600.000 (omzet rata-rata) = **10,9%**.

**Journey Adopsi:**
```
Hari 1   : Pak Yosef bermigrasi ke QRIS AstraPay dengan insentif voucher servis AHASS.
Bulan 1  : Akumulasi volume transaksi QRIS mencapai target keaktifan.
Bulan 2  : Otorisasi credit assessment alternatif tanpa agunan via Bank Saqu.
Bulan 3  : Pencairan Saku Kredit Rp20 juta disetujui (dana bersih Rp19,72 juta cair).
Bulan 4  : Pembayaran cicilan otomatis ditarik via split-settlement Rp65.556 harian.
```

---

## 3.3 Value Proposition Canvas (VPC)

### VPC Arketipe 1 — Analog Operator (Ibu Siti)

| Dimensi VPC | Detail |
|---|---|
| **Customer Jobs** | Memasak makanan, melayani transaksi secara cepat, mencatat modal operasional harian. |
| **Pains** | Antrean panjang saat warung ramai; tangan kotor/berminyak menyulitkan pencatatan di ponsel; uang bisnis dan dapur tercampur. |
| **Gains** | Uang usaha terpisah otomatis; tahu untung-rugi mingguan secara akurat; akses pinjaman modal cepat. |
| **Products & Services** | Astra-Voice Widget + QRIS AstraPay + Bank Saqu Rekening Bisnis. |
| **Pain Relievers** | Pencatatan transaksi berbasis suara (hands-free); pemisahan arus kas instan pada saat settlement QRIS. |
| **Gain Creators** | Laporan keuangan mingguan sederhana via audio notifikasi; profil kredit otomatis untuk pengajuan DANASTRA. |

### VPC Arketipe 2 — Data-Rich Underserved (Narjo)

| Dimensi VPC | Detail |
|---|---|
| **Customer Jobs** | Mengelola operasional kafe; memproyeksikan stok kopi; merencanakan pembukaan cabang baru. |
| **Pains** | Data transaksi tersimpan pasif di platform POS kasir; pengajuan kredit ekspansi tetap butuh dokumen manual. |
| **Gains** | Data kasir otomatis dikonversi jadi nilai kredit; modal kerja ekspansi cair dalam waktu singkat. |
| **Products & Services** | AstraPay Flow-SDK + Predictive Procurement Alert + FIFGROUP FINATRA. |
| **Pain Relievers** | Integrasi SDK tanpa mengganti sistem POS; digital credit scoring berbasis transaksi POS harian. |
| **Gain Creators** | AI alert volatilitas harga biji kopi & cabai (API Badan Pangan); approval pinjaman FINATRA instan. |

### VPC Arketipe 3 — Platform-Trapped (Pak Yosef)

| Dimensi VPC | Detail |
|---|---|
| **Customer Jobs** | Menjaga stabilitas usaha jus buah; mendapatkan modal kerja tambahan tanpa aset berharga. |
| **Pains** | Terkunci di platform kompetitor; tidak memiliki BPKB/sertifikat tanah untuk agunan kredit perbankan. |
| **Gains** | Mendapatkan modal kerja produktif tanpa agunan fisik; biaya operasional harian terbantu oleh ekosistem. |
| **Products & Services** | Bank Saqu Saku Kredit + Poin AstraPay (Subsidi AHASS & Listrik PLN). |
| **Pain Relievers** | Skema pinjaman tanpa agunan Saku Kredit Bank Saqu; kemudahan migrasi platform dengan insentif penukar poin. |
| **Gain Creators** | Cicilan harian otomatis yang tidak membebani cash flow; subsidi fisik operasional motor (AHASS) dan token PLN. |

---

## 3.4 Matriks Komparasi Arketipe

| Karakteristik | Arketipe 1 (Analog) | Arketipe 2 (Modern) | Arketipe 3 (Transisi) |
|---|---|---|---|
| **Profil Persona** | Ibu Siti (Warteg Mulya) | Narjo (Rolun Coffee) | Pak Yosef (Jus BMC) |
| **Omzet Harian** | Rp600.000 – Rp1.200.000 | Rp3.000.000 – Rp6.000.000 | Rp400.000 – Rp800.000 |
| **Hambatan Utama** | Resistensi antarmuka kasir fisik | Data terisolasi di server POS vendor | Ketiadaan jaminan/agunan fisik |
| **Jalur Masuk** | Aplikasi AstraPay Merchant + Voice | Modul SDK AstraPay Flow | QRIS AstraPay + Insentif Poin |
| **Produk Pembiayaan** | FIFGROUP DANASTRA | FIFGROUP FINATRA | Bank Saqu Saku Kredit |
| **Skema Agunan** | Secured (BPKB Motor) | Secured (BPKB Mobil/Properti) | Unsecured (Tanpa Agunan) |
| **Plafon Kredit** | Rp15.000.000 | Rp50.000.000 | Rp20.000.000 |
| **Dana Bersih Cair** | Rp13.160.000 | Rp47.890.000 | Rp19.720.000 |
| **Suku Bunga** | 2,5% per bulan | 14% per tahun (1,17% per bulan) | 1,5% per bulan |
| **Cicilan Harian** | Rp95.833 / hari | Rp158.389 / hari | Rp65.556 / hari |
| **Rasio Cicilan/Omzet** | ~10,6% | ~3,96% | ~10,9% |

---

## 3.5 Strategi Penetrasi Pasar: Cluster-to-Cluster

* **Fase 0 — PoC (Bulan 1-3):** Uji coba pada 40 merchant di Bojongsoang untuk validasi performa *Astra-Voice Floating Widget* dan kompatibilitas injeksi SDK pada kasir digital lokal.
* **Fase 1 — Kluster Kampus (Bulan 4-12):** Ekspansi ke 5 wilayah universitas terbesar di Jawa dengan target 10.000 merchant aktif, dipimpin oleh jaringan agen surveyor lapangan FIFGROUP setempat.
* **Fase 2 — Ekspansi Nasional (Tahun 2-3):** Membuka integrasi white-label SDK bagi vendor POS nasional dengan target akuisisi 500.000 merchant aktif di seluruh Indonesia.

---

## 3.6 Mengapa Pengguna Akan Beralih (dan Bertahan)

**Ibu Siti beralih karena:** QRIS terdaftar instan dibantu agen lapangan FIFGROUP. Pembukuan warungnya otomatis terbagi dengan rekening Bank Saqu tanpa perlu repot mencatat manual dengan tangan basah.

**Narjo beralih karena:** Data ribuan transaksi kopinya tidak lagi tersimpan sia-sia di server POS kasir, melainkan diakui secara legal (sesuai POJK 29/2024) sebagai instrumen kelayakan pembiayaan FINATRA senilai Rp50 juta untuk ekspansi cabang.

**Pak Yosef beralih karena:** Menemukan satu-satunya akses kredit usaha produktif Rp20 juta tanpa syarat jaminan fisik yang selama ini menghalangi usahanya untuk berkembang.

**Mereka semua bertahan karena:** Setiap akhir pekan, AI memberi peringatan dini harga pangan (Badan Pangan RI) secara akurat untuk memitigasi fluktuasi modal kerja mereka sebelum harga di pasar naik.

---

*File 3 dari 4 — Lanjut ke BAB4_BUSINESS_IMPACT.md*
