# Bank Saqu Ecosystem & Services

> **Sumber Analisis:** Hasil Scraping Halaman Utama, Bisnis, dan Personal [banksaqu.co.id](https://banksaqu.co.id/) pada 6 Juni 2026
> **Perusahaan:** PT Bank Saqu Indonesia (dahulu PT Bank Jasa Jakarta)
> **Afiliasi:** Astra Financial & WeLab
> **Regulasi:** Berizin & diawasi OJK dan Bank Indonesia, serta penjaminan oleh LPS

---

## 1. Konsep Utama: "Saku" (Multi-Pocket Banking)
Bank Saqu dirancang sebagai teman finansial bagi solopreneur, UMKM, dan generasi produktif melalui pembagian alokasi dana secara fleksibel. Pengguna dapat membuat **hingga 20 saku berbeda** dalam satu aplikasi untuk menghindari tercampurnya keuangan usaha dan pribadi (*Mixed-Wallet Syndrome*).

### Jenis-Jenis Saku Personal & Usaha:
1. **Saku Utama:** Rekening bawaan otomatis yang aktif setelah pembukaan akun pertama kali.
2. **Saku Transaksi:** Saku untuk memisahkan dana pengeluaran harian dan operasional agar pengawasan arus kas lebih teratur.
3. **Saku Nabung:** Saku tabungan jangka panjang yang dapat disesuaikan target pencapaiannya.
4. **Saku Booster:** Saku dengan rewards khusus yang mengintegrasikan Tabungmatic, rujukan referral, dan program loyalitas lainnya.
5. **Saku Kredit:** Saku terdedikasi untuk pengelolaan angsuran dan pinjaman.

---

## 2. Layanan Perbankan Bisnis (Bank Saqu Bisnis)
Bank Saqu menyediakan solusi internet banking terpadu untuk UMKM dan korporasi melalui portal [bsb.banksaqu.co.id](https://bsb.banksaqu.co.id).

### 2.1 Fitur Corporate Internet Banking (BSB)
- **Financial Dashboard & Account Overview:** Menyajikan visibilitas penuh real-time atas seluruh saldo akun, arus kas masuk/keluar, dan laporan mutasi rekening yang dapat diekspor (format standar SWIFT MT940).
- **Manajemen Transaksi Massal (Bulk Payment):** Melakukan transfer dana dalam volume besar secara aman.
- **Sistem Penggajian (Payroll):** Pembayaran upah karyawan secara terjadwal ke rekening Bank Saqu maupun bank lain di Indonesia.
- **Pembayaran Tagihan & Pajak:** Layanan pembayaran kewajiban usaha secara terpusat.
- **Skema Keamanan Multi-User:** Struktur persetujuan bertingkat dengan otorisasi role:
  - **Administrator:** Konfigurator sistem dan pengguna.
  - **Maker:** Pembuat instruksi transaksi.
  - **Approver:** Penyetuju transaksi.
  - **Releaser:** Eksekutif pelepas dana ke jaringan kliring.
  - Pengamanan menggunakan *Two-Factor Authentication (2FA)* berupa soft/hard token.

### 2.2 Produk Simpanan Usaha
- **Giro:** Rekening giro komersial yang mendukung fitur autodebet & standing instruction untuk transaksi bisnis otomatis via BSB.
- **Giro Premium:** Akun giro eksklusif untuk korporasi skala menengah ke atas dengan pengelolaan via cek, bilyet giro, dan limit transaksi harian besar.

### 2.3 Produk Pembiayaan Bisnis (Pinjaman)
- **Kredit Modal Kerja (KMK):** Pembiayaan jangka pendek untuk modal operasional harian, bahan baku, dan likuiditas operasional.
- **Supplier Financing:** Pembiayaan rantai pasok (*Supply Chain Financing*) untuk memperlancar arus piutang merchant dengan pemasok.
- **Kredit Investasi:** Pinjaman jangka panjang untuk membiayai aset tetap bisnis seperti pembelian mesin, renovasi tempat usaha, atau pembukaan cabang baru.

---

## 3. Fitur Keunggulan Unik (Unique Value Proposition)
- **Tabungmatic (Micro-Saving):** Fitur menabung otomatis yang membulatkan sisa kembalian dari transaksi QRIS belanja harian ke dalam Saku Booster dengan suku bunga kompetitif.
- **Busposito (Community Deposit):** Skema deposito pertama di Indonesia yang mengandalkan basis komunitas. Suku bunga deposito akan meningkat secara dinamis seiring bertambahnya jumlah anggota kelompok yang bergabung dalam Busposito tersebut.
- **Warga Get Warga (Referral Engine):** Program insentif bonus saldo instan bagi pengguna yang berhasil merekomendasikan nasabah baru untuk membuka rekening Bank Saqu.

---

## 4. Relevansi Strategis untuk Hackathon AstraPay 2026
Integrasi *AstraPay Flow* dengan Bank Saqu memberikan solusi konkret atas kriteria penilaian juri:
1. **Penyelesaian Mixed-Wallet Syndrome:** Saku Bisnis Bank Saqu menjadi wadah legal bagi merchant mikro untuk memisahkan modal usaha tanpa biaya admin pembukaan rekening tambahan.
2. **Instant Settlement (0 Detik):** API transfer Bank Saqu dapat diintegrasikan dengan *AstraPay Disbursement* untuk memastikan pencairan dana QRIS merchant masuk langsung ke rekening giro Bank Saqu seketika setelah pembayaran divalidasi.
3. **Penyaluran Modal Tertutup:** Data transaksi yang masuk melalui POS pihak ketiga (melalui SDK) dapat dijadikan dasar penilaian alternatif oleh FIFGROUP, dengan pencairan modal langsung ditransfer ke rekening Bank Saqu Merchant secara aman dan diawasi OJK.

---

# LAMPIRAN: DATA WEBSITE RESMI BANK SAQU (RAW CRAWL RESULTS)

# Informasi Komprehensif Ekosistem Bank Saqu

> **Metode Pengumpulan:** Scraped menggunakan library `Scrapling` (StealthyFetcher) pada 6 Juni 2026.
> **Lisensi & Regulasi:** PT Bank Saqu Indonesia (dahulu Bank Jasa Jakarta) berizin dan diawasi OJK serta Bank Indonesia.

## Daftar Isi
- [Homepage](#homepage)
- [Personal](#personal)
- [Bisnis](#bisnis)

---

<a id='homepage'></a>
## Homepage
> **Source:** https://banksaqu.co.id/

Layanan Perbankan Digital | Bank Saqu

Kembali

[#####](/)

* [Personal](/personal)
* [Bisnis](/bisnis)
* Promo & Partner

  + [Promo](/promo)
  + [Partner](/partnership)
* [Tentang](/about)
* Berita & Blog

  + [Berita](/newsroom)
  + [Blog](/blog)
* [Bantuan](/support)

[Download App](#download-app)

[![logo](/_next/static/media/logo-3.d71318a2.png)

![logo](/_next/static/media/logo-3.d71318a2.png)](/)

* [Personal](/personal)
* [Bisnis](/bisnis)

* [Tentang](/about)
* Promo & Partner
* Berita & Blog
* [Bantuan](/support)
* Download App

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_MOBILE_CORP_PILAR_jpg_7c62513a79/HOMEPAGE_MOBILE_CORP_PILAR_jpg_7c62513a79.jpeg)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_BSQ_CORP_PILAR_jpg_adb9480666/HOMEPAGE_BSQ_CORP_PILAR_jpg_adb9480666.jpeg)

# Melangkah Bersama Bank Saqu Majukan Bangsaku

Temukan empat pilar Bank Saqu yang akan selalu mendampingi perjalanan finansial kamu sepanjang 2026.

[Baca Detail](/blog/4-pilar-bank-saqu-2026)

![img-cover-alt](https://assets.banksaqu.co.id/Mobile_Web_SELALU_CUAN_Header_jpg_3ad3da53f4/Mobile_Web_SELALU_CUAN_Header_jpg_3ad3da53f4.jpeg)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_SELALU_CUAN_jpg_1_tttttt_073c44b299/HOMEPAGE_SELALU_CUAN_jpg_1_tttttt_073c44b299.jpeg)

# Selalu Cuan, Transaksi di Bank Saqu Makin Menguntungkan!

Transaksi apa pun jadi lebih cuan! Dapatkan voucher cashback, diskon, dan promo merchant favorit yang selalu on di Bank Saqu.

[Baca Detail](/blog/program-selalu-cuan-bank-saqu)

![img-cover-alt](https://assets.banksaqu.co.id/Mobile_Web_Selalu_Ready_Header_jpg_18c8c7e7c1/Mobile_Web_Selalu_Ready_Header_jpg_18c8c7e7c1.jpeg)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_Selalu_Ready_jpg_c0130a1541/HOMEPAGE_Selalu_Ready_jpg_c0130a1541.jpeg)

# Saku Kredit Selalu Ready untuk Berbagai Kebutuhanmu

Gunakan Saku Kredit, dapatkan limit hingga Rp30.000.000 dan tenor sampai 24 bulan!

[Baca Detail](/blog/saku-kredit-selalu-ready)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_MOBILE_c_SYBER_GG_jpg_ce8315fc49/HOMEPAGE_MOBILE_c_SYBER_GG_jpg_ce8315fc49.jpeg)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_CYBER_GG_jpg_fc3763a0c1/HOMEPAGE_CYBER_GG_jpg_fc3763a0c1.jpeg)

# Waspada Serangan Hantu Cyber!

Bank Saqu menghadirkan kampanye “Serangan Hantu Cyber”, program edukasi keamanan digital yang dikemas secara kreatif, relevan, dan mudah dipahami.

[Baca selengkapnya](/blog/hantu-cyber-bank-saqu)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_MOBILE_GG_8e13c3b992/HOMEPAGE_MOBILE_GG_8e13c3b992.jpg)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_GG_8b4afd8242/HOMEPAGE_GG_8b4afd8242.jpg)

# Good Gesture: Literasi Keuangan Inklusif Bersama Teman Tuli

Bank Saqu mengajak kamu memahami keuangan lewat cara yang lebih inklusif, empatik, dan universal, agar tidak ada yang tertinggal dari pengetahuan finansial.

[Lihat Detail](/blog/video-good-gesture-bank-saqu)

## Bank Saqu Majukan Bangsaku

![](https://assets.banksaqu.co.id/PERSONAL_54ddc3cf4d/PERSONAL_54ddc3cf4d.png)

![](https://assets.banksaqu.co.id/PERSONAL_54ddc3cf4d/PERSONAL_54ddc3cf4d.png)

## Personal

Layanan perbankan untuk memudahkan Anda mengatur keuangan dalam mendukung produktivitas.

[Pelajari Lebih Lanjut](/personal)

![](https://assets.banksaqu.co.id/BISNIS_4439273824/BISNIS_4439273824.png)

![](https://assets.banksaqu.co.id/BISNIS_4439273824/BISNIS_4439273824.png)

## Bisnis

Layanan perbankan untuk memudahkan Anda mengatur keuangan dalam menunjang kelancaran bisnis.

[Pelajari Lebih Lanjut](/bisnis)

## Kenapa Harus Bank Saqu?

![features](https://assets.banksaqu.co.id/image_255_2_25c64c9ae1/image_255_2_25c64c9ae1.jpeg)

![img-percent](https://assets.banksaqu.co.id/LOGO_ASFIN_2_636fa640f7/LOGO_ASFIN_2_636fa640f7.png)

##### Dukungan Astra Financial dan WeLab

Nikmati keunggulan layanan dan kemudahan akses ke produk keuangan dari Astra Financial serta dukungan teknologi canggih dari WeLab.

![img-percent](https://assets.banksaqu.co.id/Transaksi_Bebas_Cemas_a6f456be0b/Transaksi_Bebas_Cemas_a6f456be0b.png)

##### Transaksi aman dan bebas cemas

Tidak perlu khawatir bertransaksi karena PT Bank Saqu Indonesia telah berizin dan diawasi Otoritas Jasa Keuangan (OJK) dan Bank Indonesia (BI), serta merupakan peserta Lembaga Penjamin Simpanan (LPS).

## Kolaborasi & Partner​

Bekerja sama dengan ekosistem Astra International agar investasi dan transaksi jadi lebih mudah dan cepat.​

[Selengkapnya](/partnership)

![Ecosystem Partner](https://assets.banksaqu.co.id/CIRCLE_LOGOS_a45ed0ef75/CIRCLE_LOGOS_a45ed0ef75.png)

## Pertanyaan Populer

[Lihat Semua](/support/55)

##### Apakah PT Bank Saqu Indonesia yang memiliki Aplikasi Bank Saqu berizin dan diawasi oleh OJK dan BI serta merupakan peserta penjaminan LPS?

##### Di mana kantor pusat PT Bank Saqu Indonesia?

##### Bagaimana cara membuka akun di Aplikasi Bank Saqu?

##### Apakah aman jika saya menyimpan uang di Aplikasi Bank Saqu?

![FAQ](https://assets.banksaqu.co.id/image_315_min_d914ac702a/image_315_min_d914ac702a.png)

[![Bank Jasa Jakarta](/img/common/img_footer-logo-new.png)](https://www.bjj.co.id/)

PT Bank Saqu Indonesia berizin dan diawasi oleh [Otoritas Jasa Keuangan (OJK)](https://www.ojk.go.id/id/) dan [Bank Indonesia (BI)](https://www.bi.go.id/), serta merupakan peserta penjaminan [Lembaga Penjamin Simpanan (LPS)](https://lps.go.id/). Maksimum nilai simpanan yang dijamin LPS per nasabah per bank adalah Rp2 Miliar. Untuk mengetahui Tingkat Bunga Penjaminan LPS, silakan akses [di sini](https://apps.lps.go.id/BankPesertaLPSRate).

![TUV](/img/common/tuv-kan-white.png)

Navigasi

* [Bantuan](/support)
* [Biaya & Tarif](/fees-and-rates)
* [Suku Bunga Dasar Kredit](/interest-rate)
* [Laporan](/legal/report)

Navigasi

* [Bantuan](/support)
* [Biaya & Tarif](/fees-and-rates)
* [Suku Bunga Dasar Kredit](/interest-rate)
* [Laporan](/legal/report)

Tata Kelola Perusahaan

* [Whistleblowing System](/legal/whistleblowing-system)
* [Kebijakan APU PPT dan PPSPM](/kebijakan-apu-ppt-dan-ppspm-1)

Tata Kelola Perusahaan

* [Whistleblowing System](/legal/whistleblowing-system)
* [Kebijakan APU PPT dan PPSPM](/kebijakan-apu-ppt-dan-ppspm-1)

Cerita Bank Saqu

* [Tentang Kami](/about)
* [Berita](/newsroom)
* [Blog](/blog)

Cerita Bank Saqu

* [Tentang Kami](/about)
* [Berita](/newsroom)
* [Blog](/blog)

Legalitas

* [Syarat & Ketentuan](/legal/terms-conditions)
* [Kebijakan Privasi](/legal/privacy-policy)
* [Riplay](/legal/riplay)
* [Prosedur Pengaduan Nasabah](/legal/customer-complaint-procedure)

Legalitas

* [Syarat & Ketentuan](/legal/terms-conditions)
* [Kebijakan Privasi](/legal/privacy-policy)
* [Riplay](/legal/riplay)
* [Prosedur Pengaduan Nasabah](/legal/customer-complaint-procedure)

![TUV & KAN](/img/common/tuv-kan-white.png)

Copyright © 2026 PT Bank Saqu Indonesia. All Rights Reserved.

---

<a id='personal'></a>
## Personal
> **Source:** https://banksaqu.co.id/personal

 Layanan Perbankan Digital Personal | Bank Saqu

Kembali

[#####](/)

[Personal](/personal)[Bisnis](/bisnis)

* Simpanan

  + [Saku Utama](/products/saku-utama-35)
  + [Saku Transaksi](/products/saku-transaksi-36)
  + [Saku Nabung](/products/saku-nabung-37)
  + [Saku Booster](/products/saku-booster-11)
  + [Tabungan Jasa](/products/tabungan-jasa-)
  + [Tabungan Sejahtera](/products/tabungan-sejahtera-51)
* Investasi

  + [Deposito Reguler](/products/deposito-reguler-10)
  + [Busposito](/products/busposito-16)
* Pinjaman

  + [Saku Kredit](/products/saku-kredit-15)
  + [Kredit Kepemilikan Mobil](/products/kredit-kepemilikan-mobil-53)
  + [Kredit Kepemilikan Rumah](/products/kredit-kepemilikan-rumah-54)
* Fitur dan Kenyamanan

  + [Kartu Debit Bank Saqu](/products/kartu-debit-bank-saqu-41)
  + [Kirim & Bayar](/products/kirim--bayar-14)
  + [QRIS](/products/QRIS-59)
  + [Tabungmatic](/products/tabungmatic-19)
  + [Undang Teman](/products/undang-teman-43)
  + [Set Target](/products/set-target-61)
  + [Asuransi](/products/asuransi-55)
  + [Money Changer](/products/money-changer-56)
  + [Warga Get Warga](/products/warga-get-warga-13)

* Promo & Partner

  + [Promo](/promo)
  + [Partner](/partnership)
* [Tentang](/about)
* Berita & Blog

  + [Berita](/newsroom)
  + [Blog](/blog)
* [Bantuan](/support)

[Download App](#download-app)

[![logo](/_next/static/media/logo-3.d71318a2.png)

![logo](/_next/static/media/logo-3.d71318a2.png)](/)

* [Personal](/personal)
* [Bisnis](/bisnis)

* [Tentang](/about)
* Promo & Partner
* Berita & Blog
* [Bantuan](/support)
* Download App

![Illustration](https://assets.banksaqu.co.id/shutterstocddk_2186539973_498c6bd8bd/shutterstocddk_2186539973_498c6bd8bd.png)

###### 

# Buka Seluruh Potensi Terbaikmu dan Mulailah dengan Bank Saqu

Buka potensi terbaikmu bersama Bank Saqu, layanan perbankan yang didesain untuk semua hal produktif yang kamu lakukan.

[Download Sekarang](#download-app)

## Bank yang tepat untuk memenuhi kebutuhanmu

![icon-tab-0](https://assets.banksaqu.co.id/Frame_230_82f7d07884/Frame_230_82f7d07884.png)

Simpanan

![icon-tab-1](https://assets.banksaqu.co.id/Frame_231_93bf56205e/Frame_231_93bf56205e.png)

Investasi

![icon-tab-2](https://assets.banksaqu.co.id/Frame_232_a69fe23529/Frame_232_a69fe23529.png)

Pinjaman

![icon-tab-3](https://assets.banksaqu.co.id/Frame_233_24ef3d7ab2/Frame_233_24ef3d7ab2.png)

Fitur dan Kenyamanan

#### Simpanan

Yuk, fokus sama hal-hal yang paling penting buat kamu. Sisanya, biar Bank Saqu yang urus.

[![Image](https://assets.banksaqu.co.id/PERSONALIZED_POCKET_256d468ad6/PERSONALIZED_POCKET_256d468ad6.png)

#### Saku Utama

#### Saku Utama

Saku yang otomatis kamu punya setelah pertama kali membuat akun Bank Saqu.](/products/saku-utama-35)

[![Image](https://assets.banksaqu.co.id/PERSONALIZED_POCKET_256d468ad6/PERSONALIZED_POCKET_256d468ad6.png)

#### Saku Transaksi

#### Saku Transaksi

Saku tambahan yang membantu kamu memisah pengeluaranmu biar atur keuangan jadi lebih mudah.](/products/saku-transaksi-36)

[![Image](https://assets.banksaqu.co.id/PERSONALIZED_POCKET_256d468ad6/PERSONALIZED_POCKET_256d468ad6.png)

#### Saku Nabung

#### Saku Nabung

Saku tambahan yang berfungsi sebagai tempat menabung dan bisa disesuaikan untuk berbagai kebutuhanmu.](/products/saku-nabung-37)

[![Image](https://assets.banksaqu.co.id/Saku_Booster_60e9cd369c/Saku_Booster_60e9cd369c.png)

#### Saku Booster

#### Saku Booster

Saku unik yang bisa bikin pengalaman banking-mu lebih seru dengan beragam aktivitas lewat Tabungmatic, Warga Get Warga, dan program rewards lainnya.](/products/saku-booster-11)

[![Image](https://assets.banksaqu.co.id/Product_Image_2_092afbf957/Product_Image_2_092afbf957.png)

#### Tabungan Jasa

#### Tabungan Jasa

Rekening tabungan yang bikin hidup lebih praktis. Tarik uang kapan saja, dapat bunga harian yang kompetitif, dan bayar tagihan langsung dari satu tempat.](/products/tabungan-jasa-51)

[![Image](https://assets.banksaqu.co.id/Product_Image_3_d446ab318a/Product_Image_3_d446ab318a.png)

#### Tabungan Sejahtera

#### Tabungan Sejahtera

Produk tabungan yang dirancang untuk memenuhi kebutuhan penyimpanan dana nasabah secara aman dan andal.](/products/tabungan-sejahtera-52)

## Bank yang tepat untuk memenuhi kebutuhanmu

![icon-tab-0](https://assets.banksaqu.co.id/Frame_230_82f7d07884/Frame_230_82f7d07884.png)

### Simpanan

Yuk, fokus sama hal-hal yang paling penting buat kamu. Sisanya, biar Bank Saqu yang urus.

Segera Hadir

[![Image](https://assets.banksaqu.co.id/PERSONALIZED_POCKET_256d468ad6/PERSONALIZED_POCKET_256d468ad6.png)

#### Saku Utama](/products/saku-utama-35)

[![Image](https://assets.banksaqu.co.id/PERSONALIZED_POCKET_256d468ad6/PERSONALIZED_POCKET_256d468ad6.png)

#### Saku Transaksi](/products/saku-transaksi-36)

[![Image](https://assets.banksaqu.co.id/PERSONALIZED_POCKET_256d468ad6/PERSONALIZED_POCKET_256d468ad6.png)

#### Saku Nabung](/products/saku-nabung-37)

[![Image](https://assets.banksaqu.co.id/Saku_Booster_60e9cd369c/Saku_Booster_60e9cd369c.png)

#### Saku Booster](/products/saku-booster-11)

[![Image](https://assets.banksaqu.co.id/Product_Image_2_092afbf957/Product_Image_2_092afbf957.png)

#### Tabungan Jasa](/products/tabungan-jasa-51)

[![Image](https://assets.banksaqu.co.id/Product_Image_3_d446ab318a/Product_Image_3_d446ab318a.png)

#### Tabungan Sejahtera](/products/tabungan-sejahtera-52)

![icon-tab-1](https://assets.banksaqu.co.id/Frame_231_93bf56205e/Frame_231_93bf56205e.png)

### Investasi

Gak perlu ribet bandingin bunga investasi. Pake Bank Saqu biar uangmu bertumbuh.

Segera Hadir

[![Image](https://assets.banksaqu.co.id/Time_Deposit_Banner_f3bb520b31/Time_Deposit_Banner_f3bb520b31.png)

#### Deposito Reguler](/products/deposito-reguler-10)

[![Image](https://assets.banksaqu.co.id/V2_d8cee1737d/V2_d8cee1737d.png)

#### Busposito](/products/busposito-16)

![icon-tab-2](https://assets.banksaqu.co.id/Frame_232_a69fe23529/Frame_232_a69fe23529.png)

### Pinjaman

Biar kamu bisa makin produktif, pakai pinjaman dari Bank Saqu kalau butuh tambahan dana.

Segera Hadir

[![Image](https://assets.banksaqu.co.id/image_267_3226669c29/image_267_3226669c29.jpg)

#### Saku Kredit](/products/saku-kredit-15)

[![Image](https://assets.banksaqu.co.id/Product_Image_4_eff56dc381/Product_Image_4_eff56dc381.png)

#### Kredit Kepemilikan Mobil](/products/kredit-kepemilikan-mobil-53)

[![Image](https://assets.banksaqu.co.id/Product_Image_5_1_819cef24a7/Product_Image_5_1_819cef24a7.png)

#### Kredit Kepemilikan Rumah](/products/kredit-kepemilikan-rumah-54)

![icon-tab-3](https://assets.banksaqu.co.id/Frame_233_24ef3d7ab2/Frame_233_24ef3d7ab2.png)

### Fitur dan Kenyamanan

Nikmati kemudahan bayar dan belanja pakai kartu debit, QRIS, dan fitur lainnya biar kamu bisa fokus hidup tanpa ribet.

Segera Hadir

[![Image](https://assets.banksaqu.co.id/Frame_395640285_6129f4a375/Frame_395640285_6129f4a375.png)

#### Kartu Debit Bank Saqu](/products/kartu-debit-bank-saqu-41)

[![Image](https://assets.banksaqu.co.id/Bank_Features_Banner_6d312a5920/Bank_Features_Banner_6d312a5920.png)

#### Kirim & Bayar](/products/kirim--bayar-14)

[![Image](https://assets.banksaqu.co.id/Product_Image_11_0944667535/Product_Image_11_0944667535.png)

#### QRIS](/products/qris-59)

[![Image](https://assets.banksaqu.co.id/TABUNGMATIC_b8ab2f2c36/TABUNGMATIC_b8ab2f2c36.png)

#### Tabungmatic](/products/tabungmatic-19)

[![Image](https://assets.banksaqu.co.id/Mask_group_5_10a6bc1593/Mask_group_5_10a6bc1593.png)

#### Undang Teman](/products/undang-teman-43)

[![Image](https://assets.banksaqu.co.id/Product_Image_15_1_13b7265572/Product_Image_15_1_13b7265572.jpg)

#### Set Target](/products/set-target-61)

[![Image](https://assets.banksaqu.co.id/Product_Image_6_812da5493e/Product_Image_6_812da5493e.png)

#### Asuransi](/products/asuransi-55)

[![Image](https://assets.banksaqu.co.id/Product_Image_7_527f49a827/Product_Image_7_527f49a827.png)

#### Money Changer](/products/money-changer-56)

[![Image](https://assets.banksaqu.co.id/Mask_group_f45cccffc3/Mask_group_f45cccffc3.png)

#### Warga Get Warga](/products/warga-get-warga-13)

## Promo

[Lihat Semua](/promo)

[![Banner](https://assets.banksaqu.co.id/AZKO_SKREADY_WEB_13ef849d0e/AZKO_SKREADY_WEB_13ef849d0e.jpg)

#### AZKO

01 Apr 2026 - 30 Jun 2026](/promo/azko-450)

[![Banner](https://assets.banksaqu.co.id/INFORMA_SKREADY_WEB_afd9877acf/INFORMA_SKREADY_WEB_afd9877acf.jpg)

#### INFORMA & INFORMA ELECTRONIC

01 Apr 2026 - 30 Jun 2026](/promo/informa--informa-electronic-453)

[![Banner](https://assets.banksaqu.co.id/MCD_SKREADY_WEB_ff12ff24ce/MCD_SKREADY_WEB_ff12ff24ce.jpg)

#### McDonald’s Indonesia

01 Apr 2026 - 30 Sep 2026](/promo/mcdonalds-indonesia-190)

[![Banner](https://assets.banksaqu.co.id/Partnership_Ramadhan_GOLDEN_RAMA_af4234b7f3/Partnership_Ramadhan_GOLDEN_RAMA_af4234b7f3.jpg)

#### Golden Rama

01 Jan 2026 - 30 Jun 2026](/promo/golden-rama-443)

## Kata Warga Tentang Kami

![Image](https://assets.banksaqu.co.id/Whats_App_Image_2024_10_08_at_10_23_06_8cf7040074/Whats_App_Image_2024_10_08_at_10_23_06_8cf7040074.jpeg)

#### Steven Yoshi

Solopreneur & Owner Scent Of Pluto

![gfx](/img/home/img_home-review-gfx.svg)

##### Very helpful dan easy to use. Sangat memudahkan kita yang perlu membagi pocket untuk keperluan pribadi dan bisnis. Fitur Tabungmatic juga sangat membantu kita menabung.

![reviewer](https://assets.banksaqu.co.id/thumbnail_Whats_App_Image_2024_10_08_at_10_23_06_8cf7040074/thumbnail_Whats_App_Image_2024_10_08_at_10_23_06_8cf7040074.jpeg)

###### Steven Yoshi

Solopreneur & Owner Scent Of Pluto

![Image](https://assets.banksaqu.co.id/Whats_App_Image_2024_10_08_at_10_23_19_d22da6053a/Whats_App_Image_2024_10_08_at_10_23_19_d22da6053a.jpeg)

#### ​Ary Huang

Solopreneur & Owner Riyoka Black

![gfx](/img/home/img_home-review-gfx.svg)

##### Bank Saqu memberikan banyak manfaat. Saya sangat menikmati cashback-nya, terutama untuk membeli bahan bisnis saya. Jika bisa berhemat, kenapa tidak?

![reviewer](https://assets.banksaqu.co.id/thumbnail_Whats_App_Image_2024_10_08_at_10_23_19_d22da6053a/thumbnail_Whats_App_Image_2024_10_08_at_10_23_19_d22da6053a.jpeg)

###### ​Ary Huang

Solopreneur & Owner Riyoka Black

![Image](https://assets.banksaqu.co.id/Whats_App_Image_2024_10_08_at_10_23_38_fb39ef7096/Whats_App_Image_2024_10_08_at_10_23_38_fb39ef7096.jpeg)

#### Fadhilah

Solopreneur & Owner Manique.co

![gfx](/img/home/img_home-review-gfx.svg)

##### Pakai Bank Saqu sangat praktis. Banyak promo cashback saat event membuat pembeli tertarik belanja. Sistem saku juga sangat memudahkan kami mengelola keuangan.

![reviewer](https://assets.banksaqu.co.id/thumbnail_Whats_App_Image_2024_10_08_at_10_23_38_fb39ef7096/thumbnail_Whats_App_Image_2024_10_08_at_10_23_38_fb39ef7096.jpeg)

###### Fadhilah

Solopreneur & Owner Manique.co

* Buka Akun

## Cara Buka Akun Bank Saqu

![Image](https://assets.banksaqu.co.id/On_Boarding_1_736623a4d5/On_Boarding_1_736623a4d5.png)

![Image](https://assets.banksaqu.co.id/On_Boarding_6_b013318d89/On_Boarding_6_b013318d89.png)

![Image](https://assets.banksaqu.co.id/On_Boarding_3_67650e7dab/On_Boarding_3_67650e7dab.png)

![Image](https://assets.banksaqu.co.id/On_Boarding_4_9aceb6b8d8/On_Boarding_4_9aceb6b8d8.png)

![Image](https://assets.banksaqu.co.id/On_Boarding_6_fc0d7c44ac/On_Boarding_6_fc0d7c44ac.png)

## Cara Buka Akun Bank Saqu

* 1: #### Klik Buka Akun

  Klik Buka Akun untuk memulai proses registrasi
* 2: #### Siapkan e-KTP

  Siapkan e-KTP kamu dan agar proses pembukaan akun berjalan lancar, baca ketentuan yang tersedia.
* 3: #### Isi Nomor Ponsel dan Email

  Isi Nomor Ponsel dan Email kamu yang aktif sebagai proses verifikasi.
* 4: #### Upload e-KTP & Selfie

  Foto e-KTP kamu. Lalu lakukan Selfie. Pastikan kamu membaca panduan terlebih dahulu agar proses nya lebih mudah.
* 5: #### Pembukaan Akun Telah Berhasil

  Selamat, pembukaan akun kamu sudah berhasil. Kamu bisa menikmati seluruh fasilitas Bank Saqu untuk memudahkan seluruh urusan kamu.

1

## Klik Buka Akun

Klik Buka Akun untuk memulai proses registrasi

2

## Siapkan e-KTP

Siapkan e-KTP kamu dan agar proses pembukaan akun berjalan lancar, baca ketentuan yang tersedia.

3

## Isi Nomor Ponsel dan Email

Isi Nomor Ponsel dan Email kamu yang aktif sebagai proses verifikasi.

4

## Upload e-KTP & Selfie

Foto e-KTP kamu. Lalu lakukan Selfie. Pastikan kamu membaca panduan terlebih dahulu agar proses nya lebih mudah.

5

## Pembukaan Akun Telah Berhasil

Selamat, pembukaan akun kamu sudah berhasil. Kamu bisa menikmati seluruh fasilitas Bank Saqu untuk memudahkan seluruh urusan kamu.

## Pertanyaan Populer

Lihat Semua

##### Apakah Aplikasi Bank Saqu bisa digunakan pada semua jenis perangkat?

##### Apa bedanya saldo di Saku Utama dan Total Saldo?

##### Apakah bisa membuka akun dengan nomor HP atau email orang lain?

##### Apakah ada batas waktu untuk transfer dana?

![FAQ](https://assets.banksaqu.co.id/image_315_min_d914ac702a/image_315_min_d914ac702a.png)

[![Bank Jasa Jakarta](/img/common/img_footer-logo-new.png)](https://www.bjj.co.id/)

PT Bank Saqu Indonesia berizin dan diawasi oleh [Otoritas Jasa Keuangan (OJK)](https://www.ojk.go.id/id/) dan [Bank Indonesia (BI)](https://www.bi.go.id/), serta merupakan peserta penjaminan [Lembaga Penjamin Simpanan (LPS)](https://lps.go.id/). Maksimum nilai simpanan yang dijamin LPS per nasabah per bank adalah Rp2 Miliar. Untuk mengetahui Tingkat Bunga Penjaminan LPS, silakan akses [di sini](https://apps.lps.go.id/BankPesertaLPSRate).

![TUV](/img/common/tuv-kan-white.png)

Navigasi

* [Bantuan](/support)
* [Biaya & Tarif](/fees-and-rates)
* [Suku Bunga Dasar Kredit](/interest-rate)
* [Laporan](/legal/report)

Navigasi

* [Bantuan](/support)
* [Biaya & Tarif](/fees-and-rates)
* [Suku Bunga Dasar Kredit](/interest-rate)
* [Laporan](/legal/report)

Tata Kelola Perusahaan

* [Whistleblowing System](/legal/whistleblowing-system)
* [Kebijakan APU PPT dan PPSPM](/kebijakan-apu-ppt-dan-ppspm-1)

Tata Kelola Perusahaan

* [Whistleblowing System](/legal/whistleblowing-system)
* [Kebijakan APU PPT dan PPSPM](/kebijakan-apu-ppt-dan-ppspm-1)

Cerita Bank Saqu

* [Tentang Kami](/about)
* [Berita](/newsroom)
* [Blog](/blog)

Cerita Bank Saqu

* [Tentang Kami](/about)
* [Berita](/newsroom)
* [Blog](/blog)

Legalitas

* [Syarat & Ketentuan](/legal/terms-conditions)
* [Kebijakan Privasi](/legal/privacy-policy)
* [Riplay](/legal/riplay)
* [Prosedur Pengaduan Nasabah](/legal/customer-complaint-procedure)

Legalitas

* [Syarat & Ketentuan](/legal/terms-conditions)
* [Kebijakan Privasi](/legal/privacy-policy)
* [Riplay](/legal/riplay)
* [Prosedur Pengaduan Nasabah](/legal/customer-complaint-procedure)

![TUV & KAN](/img/common/tuv-kan-white.png)

Copyright © 2026 PT Bank Saqu Indonesia. All Rights Reserved.

---

<a id='bisnis'></a>
## Bisnis
> **Source:** https://banksaqu.co.id/bisnis

Solusi Perbankan Bisnis Modern | Bank Saqu

Kembali

[#####](/)

[Personal](/personal)[Bisnis](/bisnis)

* Simpanan

  + [Giro](/products/giro-45)
  + [Giro Premium](/products/giro-premium-46)
* Pinjaman

  + [Kredit Modal Kerja](/products/kredit-modal-kerja-47)
  + [Supplier Financing](/products/supplier-financing-49)
  + [Kredit Investasi](/products/kredit-investasi-48)
* Layanan Digital

  + [Bank Saqu Bisnis](/products/bank-saqu-bisnis-50)

* Promo & Partner

  + [Promo](/promo)
  + [Partner](/partnership)
* [Tentang](/about)
* Berita & Blog

  + [Berita](/newsroom)
  + [Blog](/blog)
* [Bantuan](/support)

[Bank Saqu Bisnis](http://bsb.banksaqu.co.id)

[![logo](/_next/static/media/logo-3.d71318a2.png)

![logo](/_next/static/media/logo-3.d71318a2.png)](/)

* [Personal](/personal)
* [Bisnis](/bisnis)

* [Tentang](/about)
* Promo & Partner
* Berita & Blog
* [Bantuan](/support)
* [Bank Saqu Bisnis](http://bsb.banksaqu.co.id)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_MOBILE_CABANG_c7cecda262/HOMEPAGE_MOBILE_CABANG_c7cecda262.jpg)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_CABANG_ca035728d6/HOMEPAGE_CABANG_ca035728d6.jpg)

# Pengumuman Penutupan Kantor Fungsi Operasional

Bank Saqu akan melakukan penutupan Kantor Fungsi Operasional (KFO) Jembatan 3 dan Sunter pada 19 Mei 2026.

[Baca Detail](/blog/informasi-kantor-fungsi-operasional)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_BISNIS_MOBILE_4a7db4f9c0/HOMEPAGE_BISNIS_MOBILE_4a7db4f9c0.jpg)

![img-cover-alt](https://assets.banksaqu.co.id/HOMEPAGE_BISNIS_1_3b24dea635/HOMEPAGE_BISNIS_1_3b24dea635.jpg)

# Mari Berkembang Bersama Majukan Bisnis Anda

Dapatkan solusi layanan perbankan sesuai kebutuhan untuk mendukung kemajuan bisnis Anda.

[Cari Tahu](/products/bank-saqu-bisnis-50)

## Majukan bisnis Anda bersama Bank Saqu

![icon-tab-0](https://assets.banksaqu.co.id/Frame_230_1_259026a2db/Frame_230_1_259026a2db.png)

Simpanan

![icon-tab-1](https://assets.banksaqu.co.id/Frame_232_1_519686e205/Frame_232_1_519686e205.png)

Pinjaman

![icon-tab-2](https://assets.banksaqu.co.id/Frame_233_1_8d0ced40ce/Frame_233_1_8d0ced40ce.png)

Layanan Digital

#### Simpanan

Pantau pemasukan dan pengeluaran bisnis lewat berbagai produk Bank Saqu.

[![Image](https://assets.banksaqu.co.id/Saku_Bisnis_Giro_79067ddbb5/Saku_Bisnis_Giro_79067ddbb5.png)

#### Giro

#### Giro

Rekening giro bisnis dengan autodebet & standing instruction, transaksi mudah kapan saja via Bank Saqu Bisnis untuk pembayaran & transfer otomatis.](/products/giro-45)

[![Image](https://assets.banksaqu.co.id/Saku_Bisnis_Giro_Premium_4bcd4504cd/Saku_Bisnis_Giro_Premium_4bcd4504cd.png)

#### Giro Premium

#### Giro Premium

Rekening giro eksklusif untuk perusahaan besar yang memberikan hasil optimal dan kemudahan transaksi pembayaran melalui cek, bilyet giro, atau internet banking Bank Saqu Bisnis](/products/giro-premium-46)

## Majukan bisnis Anda bersama Bank Saqu

![icon-tab-0](https://assets.banksaqu.co.id/Frame_230_1_259026a2db/Frame_230_1_259026a2db.png)

### Simpanan

Pantau pemasukan dan pengeluaran bisnis lewat berbagai produk Bank Saqu.

Segera Hadir

[![Image](https://assets.banksaqu.co.id/Saku_Bisnis_Giro_79067ddbb5/Saku_Bisnis_Giro_79067ddbb5.png)

#### Giro](/products/giro-45)

[![Image](https://assets.banksaqu.co.id/Saku_Bisnis_Giro_Premium_4bcd4504cd/Saku_Bisnis_Giro_Premium_4bcd4504cd.png)

#### Giro Premium](/products/giro-premium-46)

![icon-tab-1](https://assets.banksaqu.co.id/Frame_232_1_519686e205/Frame_232_1_519686e205.png)

### Pinjaman

Solusi pembiayaan untuk usaha Anda agar menjadi lebih berkembang.

Segera Hadir

[![Image](https://assets.banksaqu.co.id/Saku_Bisnis_Modal_Kerja_33a5926340/Saku_Bisnis_Modal_Kerja_33a5926340.png)

#### Kredit Modal Kerja](/products/kredit-modal-kerja-47)

[![Image](https://assets.banksaqu.co.id/Supply_Chain_Financing_d93ff98cdb/Supply_Chain_Financing_d93ff98cdb.png)

#### Supplier Financing](/products/supplier-financing-49)

[![Image](https://assets.banksaqu.co.id/Bisnis_Investasi_f229e6e5f9/Bisnis_Investasi_f229e6e5f9.png)

#### Kredit Investasi](/products/kredit-investasi-48)

![icon-tab-2](https://assets.banksaqu.co.id/Frame_233_1_8d0ced40ce/Frame_233_1_8d0ced40ce.png)

### Layanan Digital

Pantau dan atur transaksi bisnis lewat layanan Bank Saqu

Segera Hadir

[![Image](https://assets.banksaqu.co.id/Saku_Bisnis_a2f47fad25/Saku_Bisnis_a2f47fad25.png)

#### Bank Saqu Bisnis](/products/bank-saqu-bisnis-50)

## Layanan Digital

![](https://assets.banksaqu.co.id/Layanan_BSQ_Bisnis_f1813446c5/Layanan_BSQ_Bisnis_f1813446c5.jpg)

![](https://assets.banksaqu.co.id/Layanan_BSQ_Bisnis_f1813446c5/Layanan_BSQ_Bisnis_f1813446c5.jpg)

## Bank Saqu Bisnis

Kelola keuangan dengan mudah kapan saja, di mana saja lewat layanan Bank Saqu Bisnis. Cek saldo, transfer, bayar tagihan, hingga kelola investasi dalam satu akses.

[Pelajari lebih lanjut](/products/bank-saqu-bisnis-50)

## Pertanyaan Populer

[Lihat Semua](/support/72)

##### Apa saja layanan khusus Bisnis di Bank Saqu?

##### Apa syarat untuk menjadi nasabah Bank Saqu Bisnis?

##### Apa yang membedakan layanan Personal dan Bisnis?

##### Siapa saja yang bisa menjadi nasabah Bank Saqu Bisnis?

![FAQ](https://assets.banksaqu.co.id/image_315_min_d914ac702a/image_315_min_d914ac702a.png)

[![Bank Jasa Jakarta](/img/common/img_footer-logo-new.png)](https://www.bjj.co.id/)

PT Bank Saqu Indonesia berizin dan diawasi oleh [Otoritas Jasa Keuangan (OJK)](https://www.ojk.go.id/id/) dan [Bank Indonesia (BI)](https://www.bi.go.id/), serta merupakan peserta penjaminan [Lembaga Penjamin Simpanan (LPS)](https://lps.go.id/). Maksimum nilai simpanan yang dijamin LPS per nasabah per bank adalah Rp2 Miliar. Untuk mengetahui Tingkat Bunga Penjaminan LPS, silakan akses [di sini](https://apps.lps.go.id/BankPesertaLPSRate).

![TUV](/img/common/tuv-kan-white.png)

Navigasi

* [Bantuan](/support)
* [Biaya & Tarif](/fees-and-rates)
* [Suku Bunga Dasar Kredit](/interest-rate)
* [Laporan](/legal/report)

Navigasi

* [Bantuan](/support)
* [Biaya & Tarif](/fees-and-rates)
* [Suku Bunga Dasar Kredit](/interest-rate)
* [Laporan](/legal/report)

Tata Kelola Perusahaan

* [Whistleblowing System](/legal/whistleblowing-system)
* [Kebijakan APU PPT dan PPSPM](/kebijakan-apu-ppt-dan-ppspm-1)

Tata Kelola Perusahaan

* [Whistleblowing System](/legal/whistleblowing-system)
* [Kebijakan APU PPT dan PPSPM](/kebijakan-apu-ppt-dan-ppspm-1)

Cerita Bank Saqu

* [Tentang Kami](/about)
* [Berita](/newsroom)
* [Blog](/blog)

Cerita Bank Saqu

* [Tentang Kami](/about)
* [Berita](/newsroom)
* [Blog](/blog)

Legalitas

* [Syarat & Ketentuan](/legal/terms-conditions)
* [Kebijakan Privasi](/legal/privacy-policy)
* [Riplay](/legal/riplay)
* [Prosedur Pengaduan Nasabah](/legal/customer-complaint-procedure)

Legalitas

* [Syarat & Ketentuan](/legal/terms-conditions)
* [Kebijakan Privasi](/legal/privacy-policy)
* [Riplay](/legal/riplay)
* [Prosedur Pengaduan Nasabah](/legal/customer-complaint-procedure)

![TUV & KAN](/img/common/tuv-kan-white.png)

Copyright © 2026 PT Bank Saqu Indonesia. All Rights Reserved.

---

