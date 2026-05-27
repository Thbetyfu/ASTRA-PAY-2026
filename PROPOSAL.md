# AstraPay Flow-SDK: Predictive Embedded Finance and AI Business Advisor Pada Platform POS Pihak Ketiga untuk Akselerasi Keberlanjutan UMKM

---

## 👥 Tim Penyusun
*   **Thoriq Abdurrohman Taqy** - Telkom University, Bandung
*   **Grace Jessica** - Telkom University, Bandung
*   **Tahun**: 2026

---

## BAB 1: RUMUSAN MASALAH

### 1.1 Latar Belakang & Validasi Lapangan
Usaha Mikro, Kecil, dan Menengah (UMKM) berkontribusi sebesar **61% terhadap Produk Domestik Baju (PDB) Indonesia**. Saat ini, lebih dari 30 juta UMKM sudah mulai menggunakan teknologi digital. Namun, pemanfaatan digitalisasi ini sebagian besar baru sebatas alat transaksi kasir (seperti QRIS statis) dan belum menyentuh perbaikan tata kelola keuangan bisnis secara fundamental.

Berdasarkan hasil wawancara mendalam dengan **10 UMKM di sekitar kawasan kampus Telkom University (Bojongsoang)**, ditemukan tiga masalah kritis nyata di lapangan yang menghambat pertumbuhan mereka.

---

### 1.2 Empathy Map
Empathy Map berikut menggambarkan apa yang dikatakan, dipikirkan, dilakukan, dan dirasakan oleh pemilik warung tradisional di kawasan Bojongsoang:

| **SAYS (Apa yang Dikatakan)** | **THINKS (Apa yang Dipikirkan)** |
| :--- | :--- |
| • "Aplikasi kasir digital terlalu rumit dan membuang waktu saat melayani antrean mahasiswa."<br>• "Lebih praktis transfer manual ke rekening pribadi saja."<br>• "Pembeli sering mengeluh karena harus membayar biaya admin tambahan." | • "Bagaimana saya bisa tahu untung bersih kalau uang jualan dan uang harian selalu bercampur setiap hari?"<br>• "Saya ingin mengajukan pinjaman modal ke bank, tetapi syarat dokumen pembukuannya sangat rumit." |
| **DOES (Apa yang Dilakukan)** | **FEELS (Apa yang Dirasakan)** |
| • Mencampur uang hasil jualan harian dengan uang kebutuhan rumah tangga di laci kas yang sama.<br>• Melayani pembeli secepat mungkin dengan tangan basah/berminyak setelah memasak.<br>• Tidak mendaftar akun merchant resmi karena bingung dengan proses administrasi yang berbelit-belit. | • **Lelah**: Kelelahan fisik mengelola warung sendiri dan pusing menghitung kembalian uang tunai di jam sibuk.<br>• **Khawatir**: Takut modal usaha diam-diam habis terpakai untuk kebutuhan sehari-hari tanpa disadari. |

---

### 1.3 Problem Statement
> **[User]** Pemilik warung makan tradisional di kawasan kampus yang mengelola usahanya sendiri,
> **[Need]** membutuhkan sistem pencatatan keuangan dan transaksi digital yang cepat, otomatis, dan tidak mengganggu kecepatan pelayanan konsumen,
> **[Insight]** karena mereka sadar mencampur uang pribadi dan uang usaha merusak kelangsungan bisnis dan mempersulit akses modal resmi, namun enggan menggunakan aplikasi kasir konvensional karena dinilai rumit dan menyita waktu di jam sibuk.

---

### 1.4 Data Validasi Pendukung (Root-Cause Analysis)

#### A. Uang Usaha & Pribadi Bercampur (*Mixed-Wallet Syndrome*)
*   **Data Primer**: Hasil wawancara dengan pemilik *Warteg Meriah, Warteg Mulya (2 Cabang), Warteg Podom, Warteg Alwi*, dan *Laundry Permata* membuktikan mereka mencampur seluruh uang pribadi dengan omzet harian. Satu-satunya merchant yang sudah tertib menggunakan POS digital terpisah di kluster ini hanyalah *Jus BMC*.
*   **Data Sekunder**: Laporan Kementerian Koperasi & UMKM RI menyatakan **69.5% pelaku UMKM di Indonesia masih unbankable** karena kendala ketiadaan laporan pembukuan usaha yang rapi.
*   **Data Tersier**: Statistik nasional menunjukkan rendahnya mobilitas vertikal UMKM di Indonesia, di mana sekitar **97% pelaku usaha tetap tertahan di level mikro** akibat tata kelola bisnis dan arus kas yang kurang terstruktur.

#### B. Biaya Administrasi Tambahan (*Payment Friction*)
*   **Data Primer**: Di *Warung Padang Mahkota*, konsumen nontunai dipaksa melakukan transfer manual ke e-wallet DANA pribadi pemilik warung untuk menghindari biaya merchant resmi. Hal ini memicu biaya admin transaksi sebesar **Rp2.500 per transaksi** jika dikirim dari bank konvensional berbeda.
*   **Data Sekunder**: Survei Nasional Literasi dan Inklusi Keuangan (SNLIK) oleh OJK menyoroti adanya *gap* lebar antara kepemilikan alat bayar digital dengan pemahaman cara menggunakannya secara efisien.
*   **Data Tersier**: Riset Katadata Insight Center (KIC) menyebutkan biaya admin Rp2.500 menjadi salah satu alasan psikologis terbesar pedagang kecil menolak pembayaran nontunai resmi.

#### C. Data Transaksi Kasir Terkunci (*Isolated POS Data*)
*   **Data Primer**: Segmen UMKM modern di Bojongsoang seperti *Rolun Coffee, Diagram Coffee*, dan *GROI* sudah menggunakan aplikasi kasir (POS) pihak ketiga seperti *Notain* dan *Majoo*. Data riwayat penjualan yang merekam ribuan transaksi per bulan tersimpan pasif di server POS pihak ketiga tanpa nilai tambah finansial bagi pemilik usaha.
*   **Data Sekunder**: Laporan *e-Conomy SEA (Google, Temasek, Bain)* menyoroti adanya *credit gap* yang masif pada UMKM karena status *underbanked*. Laporan menegaskan data transaksi digital alternatif adalah kunci utama penilaian kelayakan modal otomatis (*automated credit scoring*).
*   **Data Tersier**: Laporan *PwC Indonesia FinTech Lending Whitepaper* menyoroti adopsi sistem *Alternative Credit Scoring* berbasis data digital dapat memitigasi risiko gagal bayar pada sektor UMKM secara terukur.

---

## BAB 2: SOLUSI ASTRAPAY FLOW ECOSYSTEM

AstraPay Flow menghadirkan **Dual-Channel Framework** (Pendekatan Dua Jalur) untuk menghapus kerumitan teknologi di warung mikro tradisional, sekaligus membuka akses data pada usaha menengah modern tanpa membebani biaya merchant.

```
                  ┌─────────────────────────────────────────┐
                  │          AstraPay Flow Engine           │
                  └────────────────────┬────────────────────┘
                                       │
                ┌──────────────────────┴──────────────────────┐
                ▼                                             ▼
     [Jalur 1: AstraPay Merchant]                  [Jalur 2: AstraPay Flow-SDK]
     - Merchant Mikro Tradisional                  - Merchant Menengah Modern
     - Pendaftaran Kilat via Agen                  - Integrasi Ringan ke POS Eksis
     - Astra-Voice Widget (Tombol Suara)           - Jembatan Data QRIS CPM (Real-Time)
     - Pemisahan Uang Otomatis (Bank Saqu)         - Penilaian Kredit Otomatis
```

### 2.1 Dua Jalur Integrasi (Dual-Channel Framework)

#### 🚀 Jalur 1: Aplikasi AstraPay Merchant (UMKM Mikro Tradisional)
1.  **Pendaftaran Kilat via Agen (Zero-Friction Onboarding)**: Pendaftaran dibantu langsung oleh surveyor/agen lapangan Astra Financial menggunakan fitur OCR KTP. Akun QRIS merchant aktif kurang dari 5 menit tanpa membebani merchant dengan biaya pendaftaran.
2.  **Tombol Suara Melayang (Astra-Voice Floating Widget)**: Memanfaatkan mikrofon ponsel Android. Pedagang cukup mengetuk tombol melayang di layar kasir lalu berucap secara alami:
    > *"Astra, paket ayam telur dadar es teh, 22 ribu cash, uangnya 50 ribu."*
    Sistem otomatis mencatat transaksi ke sistem pembukuan dan menyuarakan kembalian:
    > *"Dicatat Bos, kembalian 28 ribu."*
3.  **Pemisahan Uang Otomatis**: Dana QRIS otomatis dialirkan langsung ke rekening bisnis mitra di **Bank Saqu (Astra Group)** secara terpisah dari dompet pribadi pemilik.

#### 🔌 Jalur 2: AstraPay Flow-SDK (UMKM Menengah Modern)
1.  **Embedded Finance via SDK**: AstraPay tidak membuat aplikasi kasir tandingan baru. Sebaliknya, kami menyediakan SDK ringan yang diintegrasikan langsung ke platform kasir digital (POS) pihak ketiga eksis (*Notain, Majoo, Moka POS*, dsb).
2.  **Jembatan Data QRIS CPM (Customer Presented Mode)**: Saat transaksi QRIS CPM dipindai, SDK mengirimkan salinan data transaksi secara real-time ke *data lake* AstraPay untuk dianalisis sebagai alternatif credit scoring bagi FIFGROUP.

---

### 2.2 Nilai Tambah Ekosistem (Core Value Proposition)

*   **Instant Zero-Cost Settlement (Likuiditas 0 Detik)**: Hasil penjualan nontunai langsung dicairkan ke rekening Bank Saqu dalam 0 detik tanpa biaya admin penarikan (`Rp0 Admin Fee`), menjaga arus kas harian pedagang untuk belanja esok subuh.
*   **Penasihat Bisnis Pintar (AI Business Advisor)**: Mengonversi data transaksi menjadi wawasan prediktif sederhana lewat push-notification mingguan:
    > *"Halo Bos Warteg Mulya, untung bersihmu naik 12% minggu ini! Menu telur balado menyumbang margin terbesar. Berdasarkan tren Bojongsoang, harga komoditas telur diprediksi naik besok pagi. Yuk, amankan stokmu sekarang!"*
*   **Pembiayaan Berbasis Pendapatan (Revenue-Based Financing)**: Penyaluran modal FIFGROUP tanpa bunga flat bulanan yang menakutkan bagi UMKM. Cicilan dibayarkan melalui skema potong otomatis dinamis (misal 3%) dari setiap transaksi QRIS harian. Jika warung sepi atau tutup, potongan otomatis menjadi Rp0 sehingga menekan risiko *Non-Performing Loan* (NPL).
*   **Subsidi Biaya Operasional Berkelanjutan (Closed-Loop Cost Subsidy)**: Poin dari transaksi AstraPay dikonversi langsung menjadi:
    *   **Subsidi Energi**: Token listrik gratis untuk menekan biaya tetap bulanan toko.
    *   **Subsidi Armada (AHASS)**: Oli gratis dan perawatan servis bergaransi di jaringan AHASS untuk motor operasional belanja bahan baku merchant.

---

## BAB 3: TARGET USER & VALUE PROPOSITION CANVAS

### 3.1 Profil Target User (User Personas)

```carousel
### 👵 Persona Tradisional (Ibu Siti)
*   **Profil**: Pemilik Warteg tradisional (Warteg Mulya) di Bojongsoang. Mengelola warung sendirian dibantu keluarga.
*   **Karakteristik**: Berusia 48 tahun, gaptek, mengutamakan kecepatan transaksi fisik di jam sibuk makan siang mahasiswa.
*   **Hambatan Utama**: Takut uang modal habis terpakai kebutuhan harian dan malas mengetik di aplikasi kasir yang ribet.
*   **Solusi**: Aplikasi AstraPay Merchant + Fitur Suara (Astra-Voice) + Rekening Bisnis Bank Saqu.

<!-- slide -->

### 🧑 Persona Modern (Mas Budi)
*   **Profil**: Pemilik Kafe (Rolun Coffee) di dekat Universitas Telkom.
*   **Karakteristik**: Berusia 28 tahun, melek teknologi, telah menggunakan aplikasi POS modern (Majoo) dan QRIS untuk operasional bisnis.
*   **Hambatan Utama**: Data transaksi yang tumpah ruah terisolasi di server POS pihak ketiga sehingga tidak bisa dijadikan jaminan untuk meminjam modal pembukaan cabang baru.
*   **Solusi**: Integrasi AstraPay Flow-SDK ke POS eksis untuk scoring kredit instan oleh FIFGROUP.

<!-- slide -->

### 👨 Persona Transisi (Pak Andi)
*   **Profil**: Pemilik kedai Jus Buah (Jus BMC).
*   **Karakteristik**: Berusia 35 tahun, menggunakan satu platform kasir sederhana tetapi merasa terkunci oleh satu ekosistem e-wallet karena tingginya biaya admin inter-koneksi.
*   **Hambatan Utama**: Sensitif terhadap biaya admin penarikan dana harian dan ingin memperluas jangkauan alat pembayaran nontunai tanpa ribet.
*   **Solusi**: Fitur Instant Zero-Cost Settlement (0 Detik) ke Bank Saqu + Subsidi Operasional AHASS.
```

---

### 3.2 Value Proposition Canvas (VPC)

| **Customer Profile** | **AstraPay Flow Solution (Value Map)** |
| :--- | :--- |
| **Customer Jobs**<br>• Mengolah bahan makanan/produk dagang.<br>• Melayani pembeli secepat mungkin.<br>• Mencatat pemasukan harian. | **Product & Service**<br>• Aplikasi AstraPay Merchant.<br>• AstraPay Flow-SDK untuk POS Pihak Ketiga. |
| **Pains**<br>• Uang pribadi dan usaha tercampur.<br>• Risiko modal terpakai belanja harian.<br>• Administrasi pengajuan modal usaha yang rumit. | **Pain Relievers**<br>• Fitur suara instan (Astra-Voice) untuk input transaksi cepat.<br>• Pemisahan rekening bisnis otomatis di Bank Saqu.<br>• *Automated Credit Scoring* tanpa berkas dokumen kertas. |
| **Gains**<br>• Memiliki pembukuan usaha yang rapi.<br>• Pencairan omzet cepat untuk modal belanja esok hari.<br>• Biaya operasional usaha yang lebih murah. | **Gain Creators**<br>• *Instant Zero-Cost Settlement* (Likuiditas 0 Detik).<br>• AI Business Advisor (Notifikasi Analitik Prediktif).<br>• Subsidi Listrik dan Servis AHASS berbasis akumulasi poin transaksi. |

---

### 3.3 Matriks Komparasi Target Pengguna

| Karakteristik | Arketipe 1 (Tradisional) | Arketipe 2 (Modern) | Arketipe 3 (Transisi) |
| :--- | :--- | :--- | :--- |
| **Profil Riil** | Ibu Siti (Warteg Mulya) | Mas Budi (Rolun Coffee) | Pak Andi (Jus BMC) |
| **Hambatan Utama** | Resisten terhadap aplikasi yang rumit | Data transaksi terisolasi di pihak ketiga | Terkunci di satu platform karena biaya admin |
| **Solusi Utama** | Widget Suara (Astra-Voice) | Injeksi SDK AstraPay Flow | Settlement 0 Detik & Insentif AHASS |
| **Jalur Onboarding** | Registrasi langsung via Agen Lapangan | Integrasi API/SDK oleh Developer POS | Registrasi Mandiri / Online |

---

## BAB 4: BUSINESS IMPACT

```
                                  ┌──────────────────────────┐
                                  │   Volume Transaksi Naik  │
                                  └────────────┬─────────────┘
                                               │
                                               ▼
┌──────────────────────────┐      ┌──────────────────────────┐      ┌──────────────────────────┐
│  Loyalitas Ekosistem     │◀─────┤  CASA Flywheel (Saqu)    ├─────▶│  Kredit Produktif (FIF)  │
│  AHASS & Lini Otomotif   │      └──────────────────────────┘      │  NPL Rendah (< 1%)       │
└──────────────────────────┘                                        └──────────────────────────┘
```

### 4.1 Dampak Akselerasi UMKM (Sisi Eksternal)
*   **Menghentikan Financial Ghost Identity**: Pemilik usaha mikro (seperti *Warteg Podom* dan *Warteg Alwi*) bergeser memiliki laporan laba-rugi terstruktur, membuat mereka dinilai layak mendapatkan pendanaan formal.
*   **Membuka Kunci Data (Unlocking Isolated POS Data)**: Riwayat ribuan transaksi kafe (seperti *Rolun Coffee* dan *GROI*) dikonversi menjadi aset strategis berwujud skor kredit digital siap pakai.
*   **Mengamankan Likuiditas Arus Kas**: Melalui *Instant Zero-Cost Settlement*, turn-over kas harian meningkat sebesar **30%**, membebaskan merchant dari jeratan rentenir informal.

---

### 4.2 Dampak Sinergi Lini Bisnis Astra Group (Sisi Internal)
*   **AstraPay**: Mengakuisisi Volume Transaksi Bruto (GTV) masif dari kluster merchant kuliner high-volume di Bojongsoang yang semula dikuasai oleh kompetitor tunggal.
*   **Bank Saqu**: Mengakumulasi Dana Murah (CASA Flywheel) dengan biaya akuisisi nasabah (CAC) mendekati Rp0 karena setiap merchant diwajibkan membuka rekening penampungan bisnis di Bank Saqu.
*   **FIFGROUP**: Menyalurkan kredit produktif dengan risiko minimal (NPL < 1%) berkat mekanisme penagihan *Revenue-Based Collection* (potong otomatis harian dari QRIS).
*   **AHASS (Astra Motor)**: Meningkatkan loyalitas ekosistem fisik. UMKM akan tetap setia merawat kendaraan operasionalnya di AHASS dan menggunakan motor Honda untuk logistik belanja harian karena subsidi oli gratis berbasis transaksi AstraPay.

---

### 4.3 Analisis Kelayakan Arsitektur Finansial (LTV/CAC Ratio)
Dalam bisnis digital konvensional, rasio LTV ke CAC seringkali tidak sehat akibat promo bakar uang yang masif. AstraPay Flow menekan biaya akuisisi nasabah (CAC) melalui sinergi ekosistem tertutup:

$$\text{Sinergi CAC} = \text{Shared Network FIF Surveyor} + \text{Internal COGS AHASS}$$

*   **Pemberian oli gratis di AHASS** tidak membebani harga retail pasar (Rp50.000) kepada Astra, melainkan menggunakan perhitungan *Cost of Goods Sold* (COGS) internal manufaktur asli Astra yang jauh lebih rendah (sekitar Rp15.000).
*   **On-Us Network Likuiditas**: Aliran dana dari dompet digital ke Bank Saqu diselesaikan secara internal, menekan biaya kliring antar-bank menjadi Rp0.

---

## BAB 5: BUSINESS MODEL CANVAS & KPI

### 5.1 Business Model Canvas (BMC)

| **Key Partners** | **Key Activities** | **Value Propositions** | **Customer Relationships** | **Customer Segments** |
| :--- | :--- | :--- | :--- | :--- |
| • **AstraPay** (Payment Platform)<br>• **Bank Saqu** (CASA Provider)<br>• **FIFGROUP** (Credit Engine)<br>• **AHASS & Astra Motor** (Subsidy Partner)<br>• **Developer POS Pihak Ketiga** (Notain, Majoo, dsb) | • Integrasi & Pemeliharaan SDK.<br>• Pengembangan Fitur Suara (Astra-Voice AI).<br>• Manajemen Distribusi Poin & Subsidi Operasional. | **Untuk UMKM Mikro**: Pencatatan keuangan bebas ribet menggunakan suara, pemisahan kas harian instan.<br><br>**Untuk UMKM Modern**: Integrasi kasir eksis tanpa ganti sistem, pembukaan skor kredit instan.<br><br>**Untuk Perusahaan**: Akuisisi data alternatif, dana murah CASA, & NPL kredit produktif rendah. | • *Self-Service App Platform* untuk merchant modern.<br>• *Personal Assistance* (Agen Lapangan) untuk onboarding merchant tradisional.<br>• AI Advisor Push-Notification interaktif mingguan. | • **Arketipe 1**: Pedagang Mikro Tradisional (Warteg, kelontong, dsb).<br>• **Arketipe 2**: Pemilik Usaha Menengah Modern (Kafe, laundry modern, dsb).<br>• **Arketipe 3**: Pedagang Transisi (Jus buah, warung kopi kecil). |
| | **Key Resources** | | **Channels** | |
| | • Infrastruktur SDK & AI Voice API.<br>• Jaringan Surveyor Lapangan FIFGROUP.<br>• Jaringan Bengkel Resmi AHASS. | | • Aplikasi AstraPay Merchant & Customer.<br>• Dashboard POS Pihak Ketiga terintegrasi SDK.<br>• Jaringan Agen Lapangan Astra Financial. | |
| **Cost Structure** | **Revenue Streams** |
| • Biaya operasional pemeliharaan API & AI Voice Recognition.<br>• Biaya subsidi token listrik dan oli/servis motor AHASS (berbasis COGS internal).<br>• Biaya insentif komisi integrasi developer POS pihak ketiga. | • Pendapatan bunga/bagi-hasil dari penyaluran kredit produktif FIFGROUP (Revenue-Based Financing).<br>• Spread margin/bunga dari pengendapan dana CASA merchant di Bank Saqu.<br>• Biaya administrasi minimal (MDR) dari transaksi QRIS merchant segmen menengah. |

---

### 5.2 SMART KPIs

Berikut adalah 3 KPI Utama untuk melacak tingkat keberhasilan implementasi program AstraPay Flow:

| KPI | S (Specific) | M (Measurable) | A (Achievable) | R (Relevant) | T (Time-Bound) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Aktivasi Merchant Mikro** | Peningkatan pendaftaran merchant mikro tradisional baru di kluster Bojongsoang. | Target minimal **50 merchant baru** terdaftar dan aktif menggunakan QRIS AstraPay. | Dapat dicapai karena didukung oleh tim surveyor lapangan regional FIFGROUP yang siap bergerak. | Sangat relevan untuk meningkatkan volume transaksi retail AstraPay dan dana simpanan Bank Saqu. | Harus dicapai dalam waktu **6 bulan pertama** peluncuran program. |
| **Penurunan Tingkat NPL Kredit** | Menjaga tingkat kegagalan bayar kredit produktif UMKM yang disalurkan FIFGROUP. | Tingkat NPL diproyeksikan tertahan **di bawah 1.0%** dari total penyaluran dana. | Dapat dicapai berkat skema bagi hasil dinamis harian (3% cut) otomatis dari pendapatan QRIS. | Relevan untuk membuktikan keandalan sistem *Alternative Credit Scoring* dan mitigasi risiko. | Diukur secara berkelanjutan setiap kuartal (**Quarterly Evaluation**). |
| **Integrasi POS Pihak Ketiga** | Sukses melakukan injeksi SDK AstraPay Flow pada aplikasi kasir POS mitra. | Minimal **3 Platform POS besar** (misal Notain, Majoo, Olsera) resmi terintegrasi SDK. | Dapat dicapai dengan insentif komisi dan peningkatan transaksi bagi developer POS. | Relevan untuk menjaring Volume Transaksi Bruto (GTV) segmen modern (kafe urban). | Harus diselesaikan dalam waktu **12 bulan pertama** (Rencana Fase 1). |

---

### 5.3 Tabel Area Fokus & Diskusi Aliran Pendapatan (*Revenue Streams*)

| **Area Fokus** | **Pertanyaan Diskusi Utama** | **Output yang Diharapkan** |
| :--- | :--- | :--- |
| **1. Pengendapan Likuiditas (CASA)** | Bagaimana cara menstimulasi merchant agar tetap menyimpan dana usahanya di rekening Bank Saqu dibandingkan menariknya ke bank konvensional lain? | Pemberian tingkat suku bunga bisnis berjangka harian yang kompetitif, serta kemudahan transfer keluar gratis bersyarat setelah volume saldo rata-rata bulanan terpenuhi. |
| **2. Bagi Hasil Kredit Produktif (RBF)** | Berapa persentase potong harian (*Revenue-Based Sharing*) yang paling ideal agar tidak membebani cashflow harian warung mikro tradisional di saat sepi? | Penentuan batas rentang potongan harian yang dinamis antara **1% s.d. 5%** tergantung kategori margin bisnis merchant (ditentukan otomatis oleh AI Advisor). |
| **3. Monetisasi Data Kasir (SDK)** | Apakah developer POS pihak ketiga akan bersedia mengintegrasikan SDK secara gratis, ataukah diperlukan skema bagi hasil biaya transaksi (*transaction fee-split*)? | Penyusunan draf kerja sama bagi hasil senilai **0.1% s.d. 0.2%** dari MDR transaksi bagi developer POS sebagai insentif integrasi sistem yang berkelanjutan. |
| **4. Konversi Poin ke AHASS/Listrik** | Bagaimana menghitung nilai konversi transaksi QRIS ke dalam poin agar biaya subsidi fisik (AHASS/Listrik) tetap tertutup oleh laba dari RBF dan CASA? | Rumus simulasi rasio penukaran poin di mana 1 poin bernilai Rp10, yang baru bisa ditukarkan setelah merchant mencapai minimal **50 transaksi digital** per bulan. |
