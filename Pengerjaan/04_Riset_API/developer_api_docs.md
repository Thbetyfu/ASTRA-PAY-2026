# Website Dokumentasi Developer & API AstraPay (SNAP BI)

> **Sumber Resmi:** [astrapay.com/astrapay-bisnis](https://www.astrapay.com/astrapay-bisnis) & [docs.astrapay.com](https://docs.astrapay.com/)
> **Standardisasi Regulator:** Bank Indonesia SNAP BI (Standar Nasional Open API Pembayaran)
> **Jenis Layanan Teknis:** B2B Integration Services

---

## 1. Portal Utama Bisnis & Merchant
1. **Portal Bisnis Utama:** [https://www.astrapay.com/astrapay-bisnis](https://www.astrapay.com/astrapay-bisnis)
   - Portal untuk registrasi merchant baru, pengenalan solusi pembayaran digital AstraPay, serta akses ke dashboard Merchant.
2. **Dokumentasi Merchant & API Developer:** [https://docs.astrapay.com/](https://docs.astrapay.com/)
   - Menyediakan spesifikasi lengkap integrasi teknis, *sandbox testing*, manajemen sertifikat SSL, dan API endpoint untuk merchant.

---

## 2. Standardisasi Layanan Keamanan SNAP BI
AstraPay mengadopsi standar **SNAP BI (Standar Nasional Open API Pembayaran)** dari Bank Indonesia untuk menjamin interoperabilitas aman antar sistem keuangan.

### 2.1 Mekanisme Otentikasi Keamanan (X-SIGNATURE)
Sistem keamanan SNAP BI di AstraPay menggunakan kombinasi enkripsi asimetris dan enkripsi simetris:

#### 1. Signature Auth (Otentikasi Token B2B)
Digunakan untuk melakukan *request* B2B Access Token. 
- **Metode Hashing:** SHA-256 dengan enkripsi asimetris RSA-2048 menggunakan Private Key Merchant yang di-encode ke Base64.
- **Format String To Sign:**
  ```text
  [X-CLIENT-KEY] + "|" + [X-TIMESTAMP]
  ```
- **HTTP Header Header Required:**
  - `Content-Type`: `application/json`
  - `X-TIMESTAMP`: Waktu lokal Merchant (`yyyy-MM-ddTHH:mm:ssTZD`)
  - `X-CLIENT-KEY`: Client ID Merchant dari AstraPay
  - `X-SIGNATURE`: Hasil enkripsi asimetris signature auth

#### 2. API B2B Access Token Request
- **URL Endpoint:** `/snap-service/snap/v1.0/access-token/b2b`
- **Verb/Method:** `POST`
- **Body Request:** `{"grantType": "client_credentials"}`
- **Response Token:** Mengembalikan `accessToken` Modul B2B dengan durasi kedaluwarsa standar **900 Detik (15 Menit)** dan tipe token `Bearer`.

#### 3. Signature Service (Otentikasi Transaksi/Data Service)
Digunakan di setiap transaksi layanan (seperti pembayaran, cek saldo, dll) setelah mendapatkan Access Token.
- **Metode Hashing:** HMAC-SHA512 (simetris) dengan menggunakan *Client Secret* dari AstraPay sebagai Key, kemudian di-encode ke Base64.
- **Format String To Sign:**
  ```text
  [HTTP METHOD] + ":" + 
  [RELATIVE PATH URL] + ":" + 
  [B2B ACCESS TOKEN] + ":" + 
  LowerCase(HexEncode(SHA-256(Minify([HTTP BODY])))) + ":" + 
  [X-TIMESTAMP]
  ```
  *(Catatan: Untuk metode GET yang tidak memiliki body, HTTP Body diganti dengan JSON kosong `{}` sebelum di-hash).*

---

## 3. Jenis Integrasi API Utama (Real-Time Services)

### 3.1 Payment Channel (Direct Debit / Web Payment)
- **Fungsi:** Mengintegrasikan dompet digital AstraPay sebagai sumber dana langsung (*source of funds*) di platform e-commerce, web bisnis, atau aplikasi kasir (POS).
- **Alur Kerja:** Mengikat akun (*account binding*), verifikasi OTP, dan debet saldo otomatis secara instan.

### 3.2 Merchant QRIS (MPM & CPM)
- **Merchant Presented Mode (MPM):** 
  - *Dynamic QRIS:* API menghasilkan kode QR dinamis unik sesuai nominal transaksi secara real-time. Merchant menampilkannya di layar POS.
  - *Static QRIS:* QRIS dicetak fisik di meja kasir.
- **Customer Presented Mode (CPM):** 
  - Kasir memindai kode QR/Barcode yang ditampilkan di aplikasi AstraPay pelanggan menggunakan pemindai barcode pada POS untuk penyelesaian transaksi super cepat.

### 3.3 AstraPay Disbursement (Bulk Transfer)
- **Fungsi:** Pengiriman dana massal secara terprogram ke banyak rekening tujuan (rekening bank maupun e-wallet) dalam satu kali pemanggilan API.
- **Kasus Penggunaan:** Penggajian karyawan (*payroll*), penyelesaian dana merchant (*settlement*), pencairan pinjaman FIFGROUP, atau distribusi insentif promo.
