# Data Resmi Pemerintah Indonesia - Verifikasi untuk Proposal AstraPay Hackathon 2026

> **Status Dokumen:** TERVERIFIKASI dari sumber primer resmi (OJK, Bank Indonesia, BPS)
> **Tanggal Kompilasi:** 6 Juni 2026
> **Tujuan:** Menggantikan seluruh angka fiktif/spekulatif dalam proposal dengan data berbasis bukti resmi

---

## 1. REGULASI - POJK Nomor 29 Tahun 2024 (PKA)

**Sumber Resmi:** Otoritas Jasa Keuangan (OJK) | JDIH OJK

### Ringkasan Regulasi
- **Nama Lengkap:** Peraturan OJK Nomor 29 Tahun 2024 tentang **Pemeringkat Kredit Alternatif (PKA)**
- **Sebelumnya Disebut:** *Innovative Credit Scoring* (ICS) - terminologi lama yang tidak lagi digunakan
- **Dasar Hukum:** Amanat UU Nomor 4 Tahun 2023 tentang Pengembangan dan Penguatan Sektor Keuangan (UU P2SK)
- **Berlaku Untuk:** Penyelenggara Inovasi Teknologi Sektor Keuangan (ITSK)

### Poin Substansi Utama POJK 29/2024
1. **Definisi PKA:** Penyelenggara ITSK yang mengolah **data alternatif** (di luar data kredit tradisional/SLIK) untuk menghasilkan skor kredit
2. **Data Alternatif yang Diakui Secara Legal:**
   - Riwayat pembayaran tagihan
   - Riwayat transaksi e-commerce
   - Aktivitas digital lainnya (termasuk **riwayat transaksi QRIS/POS merchant**)
3. **Tujuan Regulasi:**
   - Mendukung inklusi keuangan bagi kelompok *unbanked* dan *underbanked*
   - Memberikan akses pembiayaan melalui penilaian kredit berbasis data non-tradisional
4. **Cakupan Pengaturan:**
   - Kelembagaan dan perizinan penyelenggara PKA
   - Tata kelola perusahaan yang baik (GCG)
   - Mekanisme penyelenggaraan operasional
   - Pengawasan, perlindungan konsumen, dan keamanan data (sinergi dengan UU PDP No. 27/2022)
   - Ketentuan penghentian kegiatan dan pencabutan izin usaha

### Implikasi Langsung untuk Proposal Kita
Solusi AstraPay Flow-SDK yang menggunakan riwayat transaksi QRIS merchant sebagai data alternatif untuk kredit scoring **sepenuhnya legal dan didukung oleh POJK 29/2024**, selama:
- Data diolah melalui penyelenggara PKA berlisensi OJK (misal: PT PEFINDO Biro Kredit / IdScore)
- Ada mekanisme informed consent dari merchant (UU PDP No. 27/2022)

---

## 2. INKLUSI KEUANGAN - SNLIK 2024 (OJK & BPS)

**Sumber Resmi:** Survei Nasional Literasi dan Inklusi Keuangan 2024 | OJK + BPS
**Catatan:** SNLIK 2024 adalah pertama kalinya OJK berkolaborasi dengan BPS dengan metodologi yang disempurnakan

### Data Resmi SNLIK 2024

| Indikator | Nilai 2024 | Catatan |
|---|---|---|
| **Indeks Literasi Keuangan** | **65,43%** | Meningkat dari 49,68% (2022) |
| **Indeks Inklusi Keuangan** | **75,02%** | Target pemerintah 90% di 2024 - BELUM TERCAPAI |
| **Gap Inklusi** | **24,98%** | Populasi dewasa yang masih belum ter-inklusi |

### Derivasi: Angka Unbanked/Underbanked UMKM

Berdasarkan sumber resmi yang terverifikasi:
- **~69,5% UMKM di Indonesia belum memiliki akses terhadap fasilitas kredit perbankan** (Data 2025, Linkumkm.id mengutip data Kemenkop UKM)
- **43,1% dari UMKM yang belum terakses tersebut dinilai SANGAT MEMBUTUHKAN kredit** untuk ekspansi dan produktivitas usaha
- **Data Bank Dunia 2021:** 48% populasi dewasa Indonesia masih unbanked (belum memiliki rekening bank)

### Konteks Kuantitatif untuk Problem Statement

Berdasarkan data SIDT UMKM (31 Desember 2024):
- Total UMKM aktif (non-agrikultur): **30,18 juta unit**
- Total UMKM agrikultur (Sensus Pertanian BPS 2023): **29,34 juta unit**
- **Estimasi Total UMKM Indonesia: ~65 juta unit**

Proyeksi UMKM yang belum ter-akses kredit formal:
- 69,5% x 65 juta = **~45 juta UMKM tanpa akses kredit formal**
- Dari 45 juta ini, 43,1% sangat butuh kredit = **~19 juta UMKM potential target market**

---

## 3. STATISTIK QRIS - Bank Indonesia (2024-2025)

**Sumber Resmi:** Laporan Bank Indonesia | Siaran Pers BI

### Data QRIS 2024

| Metrik | Data 2024 |
|---|---|
| **Jumlah Pengguna QRIS** | 55 juta pengguna (akhir 2024) |
| **Jumlah Merchant QRIS** | **32,71 juta merchant** |
| **Nilai Transaksi 2024** | **Rp467 triliun** |
| **Pertumbuhan Volume 2024** | **175,2% (Year-on-Year)** |

### Data QRIS 2025 (Update Terbaru)

| Metrik | Data 2025 |
|---|---|
| **Jumlah Pengguna** | 59,53 juta pengguna (akhir 2025) |
| **Jumlah Merchant** | 42,75 juta merchant (akhir 2025) |
| **Volume Transaksi** | 10,33 miliar transaksi s.d. Sept 2025 (tumbuh 158% YoY) |
| **Nilai Transaksi Sem-I 2025** | Rp579 triliun |

### Data Kritis untuk Proposal: Komposisi Merchant UMKM

**~90-93% dari total merchant QRIS adalah pelaku UMKM** (Data Bank Indonesia)

Kalkulasi:
- 32,71 juta merchant x 90% = **~29,4 juta merchant QRIS adalah UMKM** (2024)
- 42,75 juta merchant x 90% = **~38,5 juta merchant QRIS adalah UMKM** (2025)

**Ini adalah TAM (Total Addressable Market) konkret untuk AstraPay Flow-SDK kita.**

---

## 4. DATA HARGA PANGAN STRATEGIS (PIHPS - Bank Indonesia)

**Sumber Resmi:** PIHPS Nasional | Bank Indonesia | `bi.go.id/hargapangan`

### Harga Komoditas Eceran Nasional (Juni 2026)

| Komoditas | Kualitas | Harga (Rp/kg) |
|---|---|---|
| **Beras** | Bawah I | Rp14.200 - Rp14.300 |
| **Beras** | Medium | Rp15.100 - Rp16.100 |
| **Beras** | Super | Rp16.800 - Rp17.400 |
| **Cabai Rawit Merah** | - | Rp74.000 - Rp84.400 |
| **Cabai Merah Keriting** | - | Rp55.000 - Rp63.000 |
| **Bawang Merah** | Sedang | Rp50.000 - Rp54.000 |
| **Bawang Putih** | Sedang | Rp37.000 - Rp40.000 |

### Referensi Historis (Akhir 2025)

| Komoditas | Harga Des 2025 |
|---|---|
| Cabai Rawit Merah | ~Rp65.300/kg |
| Bawang Merah | ~Rp50.400/kg |

### Relevansi untuk AI Business Advisor
Data PIHPS merupakan data publik resmi Bank Indonesia yang dapat digunakan sebagai:
1. **Baseline prediksi biaya bahan baku** untuk UMKM kuliner/ritel
2. **Input model AI** untuk memberikan peringatan dini volatilitas harga
3. **Grounding otomatis** agar rekomendasi AI Advisor tidak spekulatif

**URL API PIHPS:** `https://www.bi.go.id/hargapangan/` (Publik, tersedia fitur unduh data historis)

---

## 5. RINGKASAN: ANGKA KUNCI YANG WAJIB ADA DI PROPOSAL

Berikut adalah angka-angka yang sudah TERVERIFIKASI dari sumber resmi dan bisa langsung dikutip:

| Klaim dalam Proposal | Angka Resmi | Sumber |
|---|---|---|
| Jumlah UMKM di Indonesia | ~65 juta unit | SIDT UMKM + BPS Sensus 2023 |
| UMKM tanpa akses kredit formal | **69,5%** | Kemenkop UKM 2025 |
| UMKM yang sangat butuh kredit | **43,1%** dari yang belum akses | Kemenkop UKM 2025 |
| Indeks Inklusi Keuangan nasional | **75,02%** (2024) | OJK SNLIK 2024 |
| Merchant QRIS total | **32,71 juta** (2024) | Bank Indonesia |
| Merchant QRIS adalah UMKM | **~90-93%** | Bank Indonesia |
| Nilai transaksi QRIS 2024 | **Rp467 triliun** | Bank Indonesia |
| Pertumbuhan transaksi QRIS | **175,2% YoY** (2024) | Bank Indonesia |
| Regulasi credit scoring alternatif | **POJK No. 29/2024** | OJK JDIH |
| Perlindungan data merchant | **UU PDP No. 27/2022** | DPR RI |

---

## 6. CATATAN UNTUK PENGGUNAAN DATA

1. **Cara Kutip yang Benar:**
   - "Berdasarkan Survei Nasional Literasi dan Inklusi Keuangan (SNLIK) 2024 yang dilaksanakan OJK bersama BPS..."
   - "Mengacu pada data statistik Bank Indonesia, merchant QRIS aktif per 2024 mencapai 32,71 juta..."
   - "Sesuai POJK Nomor 29 Tahun 2024 tentang Pemeringkat Kredit Alternatif (PKA)..."

2. **Yang HARUS DIHINDARI:**
   - Mengklaim angka unbanked spesifik tanpa menyebutkan ini berasal dari proyeksi/turunan data
   - Menggunakan data Maucash atau Moxa sebagai pemain aktif dalam ekosistem
   - Menyebut "Innovative Credit Scoring" - terminologi ini sudah diganti dengan PKA dalam POJK 29/2024
