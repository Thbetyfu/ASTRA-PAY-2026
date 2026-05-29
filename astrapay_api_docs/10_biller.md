# Biller


## 1. Introduction Biller

Selamat datang di dokumentasi AstraPay Biller.

Dokumentasi ini menjelaskan *procedur acceptance* untuk implementasi API produk Biller dari perspektif Merchant.

Berikut adalah alur gambaran proses keseluruhan API secara umum:
!["Sequence Diagram mengakses Produk Biller secara umum"](./assets/sequence-diagram-biller-open-api-8a527d75.jpg)

### 1.1 Glosarium

Sebelum melakukan integrasi, mari kita bahas terlebih dahulu beberapa definisi dari istilah yang akan muncul pada dokumentasi ini. Penjelasan dari istilah tersebut adalah sebagai berikut:


| Istilah | Deskripsi |
| --- | --- |
| Merchant | Pihak Ketiga yang ingin melakukan integrasi dengan AstraPay |
| API | Application Programming Interface |
| Database | Kumpulan data yang telah terorganisasi dan terstruktur |
| User | Pengguna atau Customer Jasa Merchant |


## 2. Inquiry

API pada bagian ini digunakan untuk melakukan inquiry.

### 2.1 Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | POST |
| URL Sandbox | /v1/mitra-service/inquiries |


### 2.2 Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk mengakses API AstraPay hasil dari generate Signature Service |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| Authorization | String | Mandatory | Bearer token hasil generate dari [API Access Token B2B](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |


### 2.3 Request Body

**contoh cURL request body pada produk biller BPJS Kesehatan**

```shell
curl --location 'https://sandbox.astrapay.com/v1/mitra-service/inquiries' \
--header 'x-partner-id: f37e5edf-8d99-40ab-b8f0-f0f0f12f41b5' \
--header 'x-timestamp: 2011-12-03T10:15:30+07:00' \
--header 'x-signature: 1111111' \
--header 'x-external-id: 111111' \
--header 'Authorization: Bearer DlHTC8U5urS6VDsWkNMv3ealeldgjR8H5CvYYfD8n5xxx' \
--header 'Content-Type: application/json' \
--data '{
    "beneficiaryNumber": "8888802883963273",
    "productCode": "ASPOBPJ0000",
    "periodStart": "2024-03-07",
    "periodEnd": "2024-03-07"   
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| beneficiaryNumber | String | Mandatory | Data yang diinput di form field (Contoh: Nomor telepon, Token listrik, dst.) |
| productCode | String | Mandatory | Kode unik pada tiap produk |
| periodStart | String | Mandatory pada produk BPJS dan PBB | **BPJS**: Rentang waktu batasan awal yang dimulai pada saat melakukan inquiry/tanggal melakukan inquiry; **PBB**: Rentang tahun dalam melakukan inquiry |
| periodEnd | String | Mandatory pada produk BPJS | Rentang waktu batasan akhir dalam melakukan inquiry untuk mendapatkan jumlah bulan yang ingin dibayar |


### 2.4 Response Body

**Contoh Response pada produk BPJS Kesehatan**

```shell
{
    "id": 3027,
    "type": "BPJS_KESEHATAN",
    "productCode": "ASPOBPJ0000",
    "beneficiaryNumber": "8888802883963273",
    "farePrice": 1750000.00,
    "minimumPayment": 1752500.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 1752500.00,
    "margin": 0.00,
    "grandTotal": 1752500.00,
    "message": null,
    "createdAt": "2025-03-05T15:14:58.316927",
    "detailAdditionalData": [
        {
            "key": "transactionType",
            "value": "BPJS Kesehatan",
            "label": "Jenis Pembayaran"
        },
        {
            "key": "beneficiaryNumber",
            "value": "8888802883963273",
            "label": "No. BPJS Kesehatan"
        },
        {
            "key": "beneficiaryName",
            "value": "TUMIRAH,DARSO SEMITO",
            "label": "Nama Pemilik"
        },
        {
            "key": "totalMembers",
            "value": "2 orang",
            "label": "Anggota Keluarga"
        },
        {
            "key": "totalPeriods",
            "value": "1 bulan",
            "label": "Periode Bayar"
        },
        {
            "key": "basicPrice",
            "value": "Rp1.750.000",
            "label": "Jumlah Tagihan"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| id | Int | Mandatory | Kode unik hasil inquiry |
| type | String | Mandatory | Kode untuk kategori produk (Contoh: PULSA) |
| productCode | String | Mandatory | Kode untuk produk (Contoh: PUPRTEL011K) |
| minimumPayment | Int | Mandatory | Jumlah pembayaran terendah yang harus dibayarkan |
| serviceCharge | Int | Mandatory | Biaya layanan yang dikenakan kepada Merchant |
| totalPrice | Int | Mandatory | Nominal harga jual dari AstraPay (Rumus: *farePrice + serviceCharge - discount*) |
| farePrice | Int | Mandatory | Harga pokok sebelum penambahan biaya layanan, diskon, atau biaya tambahan lainnya |
| discount | Int | Mandatory | Potongan harga yang diberikan oleh AstraPay |
| margin | Int | Mandatory | Keuntungan yang didapatkan Merchant |
| grandTotal | Int | Mandatory | Nominal harga jual yang ditambahkan margin (Rumus: *totalPrice + margin*) |
| message | String | Optional | Pesan yang dimunculkan ketika terjadi error |
| status | String | Mandatory | Status Transaksi (SUCCESS / PENDING / VOID ) |
| paymentType | String | Mandatory | Pembayaran dengan nominal yang sesuai (STATIC) dan pembayaran yang dapat ditentukan jumlah nominal yang ingin dibayar (DYNAMIC) |
| detailAdditionalData | Object | Mandatory | Data tambahan dari inquiry |
| detailAdditionalData.key | String | Optional | Kata kunci untuk penunjuk data |
| detailAdditionalData.value | String | Optional | Response data |
| detailAdditionalData.label | String | Optional | Label keterangan data |


## 3. Transaksi

API pada bagian ini digunakan untuk melakukan transaksi.

### 3.1 Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | POST |
| URL Sandbox | /v1/mitra-service/transactions |


### 3.2 Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk mengakses API AstraPay hasil dari generate Signature Service |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| Authorization | String | Mandatory | Bearer token hasil generate dari [API Access Token B2B](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |


### 3.3 Request Body

**contoh cURL request body pada produk biller BPJS Kesehatan**

```shell
curl --location 'https://sandbox.astrapay.com/v1/mitra-service/transactions' \
--header 'x-partner-id: f37e5edf-8d99-40ab-b8f0-f0f0f12f41b5' \
--header 'x-timestamp: 2020-01-01T00:00:00+07:00' \
--header 'x-signature: aa' \
--header 'x-external-id: aa' \
--header 'Authorization: Bearer DlHTC8U5urS6VDsWkNMv3ealeldgjR8H5CvYYfD8n5xxx' \
--header 'Content-Type: application/json' \
--data '{
    "inquiryId": "1422",
    "partnerRefenceNo": "1A23F45"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| inquiryId | String | Mandatory | Kode unik hasil inquiry untuk transaksi |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |


### 3.4 Response Body

**Contoh Response pada produk BPJS Kesehatan**

```shell
{
    "id": 5360,
    "type": "BPJS_KESEHATAN",
    "productCode": "ASPOBPJ0000",
    "transactionNumber": "INV/BIL/ASU/250305/008UERUJ12W",
    "partnerReferenceNo": "BPJS12345",
    "beneficiaryNumber": "8888802883963273",
    "farePrice": 1750000.00,
    "serviceCharge": 2500.00,
    "discount": 0,
    "totalPrice": 1752500.00,
    "margin": 0,
    "grandTotal": 1752500.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 3027,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ASU/250305/008UERUJ12W",
            "label": "No. Transaksi"
        },
        {
            "key": "referenceNumberProvider",
            "value": "920DF838B3759EC3",
            "label": "No. Referensi"
        },
        {
            "key": "transactionType",
            "value": "BPJS Kesehatan",
            "label": "Jenis Pembayaran"
        },
        {
            "key": "beneficiaryNumber",
            "value": "8888802883963273",
            "label": "No. BPJS Kesehatan"
        },
        {
            "key": "beneficiaryName",
            "value": "TUMIRAH,DARSO SEMITO",
            "label": "Nama Pemilik"
        },
        {
            "key": "totalMembers",
            "value": "2 orang",
            "label": "Anggota Keluarga"
        },
        {
            "key": "totalPeriods",
            "value": "1 bulan",
            "label": "Periode Bayar"
        },
        {
            "key": "basicPrice",
            "value": "Rp1.750.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| id | Int | Mandatory | Kode unik hasil transaksi |
| transactionNumber | Int | Mandatory | Kode unik transaksi yang di generate oleh AstraPay |
| type | String | Mandatory | Kode untuk kategori produk (Contoh: PULSA) |
| productCode | String | Mandatory | Kode untuk produk (Contoh: PUPRTEL011K) |
| serviceCharge | Int | Mandatory | Biaya layanan yang dikenakan kepada Merchant |
| totalPrice | Int | Mandatory | Nominal harga jual dari AstraPay (Rumus: *farePrice + serviceCharge - discount*) |
| margin | Int | Mandatory | Keuntungan yang didapatkan Merchant |
| farePrice | Int | Mandatory | Harga pokok sebelum penambahan biaya layanan, diskon, atau biaya tambahan lainnya |
| discount | Int | Mandatory | Potongan harga yang diberikan oleh AstraPay |
| grandTotal | Int | Mandatory | Nominal harga jual yang ditambahkan margin (Rumus: *totalPrice + margin*) |
| message | String | Optional | Pesan yang dimunculkan ketika terjadi error |
| status | String | Mandatory | Status Transaksi (SUCCESS / PENDING / VOID ) |
| inquiryId | String | Mandatory | Kode unik hasil inquiry untuk transaksi |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |
| detailAdditionalData | Object | Mandatory | Data tambahan dari inquiry |
| detailAdditionalData.key | String | Optional | Kata kunci untuk penunjuk data |
| detailAdditionalData.value | String | Optional | Response data |
| detailAdditionalData.label | String | Optional | Label keterangan data |


## 4. Mendapatkan Status

API pada bagian ini digunakan untuk mendapatkan status transaksi.

### 4.1 Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | GET |
| URL Sandbox | /v1/mitra-service/transactions/{id} |


### 4.2 Request Path Variable


| Item | Description |
| --- | --- |
| id | Kode unik hasil transaksi |


### 4.3 Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk mengakses API AstraPay hasil dari generate Signature Service |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| Authorization | String | Mandatory | Bearer token hasil generate dari [API Access Token B2B](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |


**contoh cURL request body pada produk biller BPJS Kesehatan**

```shell
curl --location 'https://sandbox.astrapay.com/v1/mitra-service/transactions/1762' \
--header 'x-partner-id: f37e5edf-8d99-40ab-b8f0-f0f0f12f41b5' \
--header 'x-timestamp: 2024-02-22T10:39:30+07:00' \
--header 'x-signature: ede31' \
--header 'x-external-id: 12d' \
--header 'Authorization: Bearer DlHTC8U5urS6VDsWkNMv3ealeldgjR8H5CvYYfD8n5xxx' \
--header 'Content-Type: application/json'
```

### 4.4 Response Body

**Contoh Response pada produk BPJS Kesehatan**

```shell
{
    "id": 5360,
    "type": "BPJS_KESEHATAN",
    "productCode": "ASPOBPJ0000",
    "transactionNumber": "INV/BIL/ASU/250305/008UERUJ12W",
    "partnerReferenceNo": "BPJS12345",
    "beneficiaryNumber": "8888802883963273",
    "farePrice": 1750000.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 1752500.00,
    "margin": 0.00,
    "grandTotal": 1752500.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 3027,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ASU/250305/008UERUJ12W",
            "label": "No. Transaksi"
        },
        {
            "key": "referenceNumberProvider",
            "value": "920DF838B3759EC3",
            "label": "No. Referensi"
        },
        {
            "key": "transactionType",
            "value": "BPJS Kesehatan",
            "label": "Jenis Pembayaran"
        },
        {
            "key": "beneficiaryNumber",
            "value": "8888802883963273",
            "label": "No. BPJS Kesehatan"
        },
        {
            "key": "beneficiaryName",
            "value": "TUMIRAH,DARSO SEMITO",
            "label": "Nama Pemilik"
        },
        {
            "key": "totalMembers",
            "value": "2 orang",
            "label": "Anggota Keluarga"
        },
        {
            "key": "totalPeriods",
            "value": "1 bulan",
            "label": "Periode Bayar"
        },
        {
            "key": "basicPrice",
            "value": "Rp1.750.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| id | Int | Mandatory | Kode unik hasil transaksi |
| transactionNumber | Int | Mandatory | Kode unik transaksi yang di generate oleh AstraPay |
| type | String | Mandatory | Kode untuk kategori produk (Contoh: PULSA) |
| serviceCharge | Int | Mandatory | Biaya layanan yang dikenakan kepada Merchant |
| productCode | String | Mandatory | Kode untuk produk (Contoh: PUPRTEL011K) |
| beneficiaryNumber | String | Mandatory | Data yang diinput di form field (Contoh: Nomor telepon, Token listrik, dst.) |
| totalPrice | Int | Mandatory | Nominal harga jual dari AstraPay |
| margin | Int | Mandatory | Keuntungan yang didapatkan Merchant |
| farePrice | Int | Mandatory | Harga pokok sebelum penambahan biaya layanan, diskon, atau biaya tambahan lainnya |
| discount | Int | Mandatory | Potongan harga yang diberikan oleh AstraPay |
| grandTotal | Int | Mandatory | Nominal harga jual yang ditambahkan margin (Rumus: *totalPrice + margin*) |
| message | String | Optional | Pesan yang dimunculkan ketika terjadi error |
| status | String | Mandatory | Status Transaksi (SUCCESS / PENDING / VOID ) |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |
| detailAdditionalData | Object | Mandatory | Data tambahan dari inquiry |
| detailAdditionalData.key | String | Optional | Kata kunci untuk penunjuk data |
| detailAdditionalData.value | String | Optional | Response data |
| detailAdditionalData.label | String | Optional | Label keterangan data |


## 5. Cek Status Transaksi

API pada bagian ini digunakan untuk melakukan pengecekan status transaksi. Untuk menggunakan API ini, pastikan setiap value partnerReferenceNo bersifat unik pada setiap transaksi yang dilakukan oleh Merchant.

### 5.1 Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | POST |
| URL Sandbox | /v1/mitra-service/transactions/status |


### 5.2 Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk mengakses API AstraPay hasil dari generate Signature Service |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| Authorization | String | Mandatory | Bearer token hasil generate dari [API Access Token B2B](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |


### 5.3 Request Body

**contoh cURL request body pada API cek status transaksi produk biller Pulsa**

```shell
curl --location 'https://sandbox.astrapay.com/v1/mitra-service/transactions/status' \
--header 'x-partner-id: 6f83d534-bd7a-48f8-bb0d-c93d58bc6312' \
--header 'x-timestamp: 2025-06-25T13:49:50+07:00' \
--header 'x-signature: CLZTCk8EUt/xxxxx==' \
--header 'x-external-id: ymPabcdelasjcjzsqewxxxxx' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIxxxxx' \
--header 'Content-Type: application/json' \
--data '{
    "partnerReferenceNo": "uQmi67EughqXhzLovTcmAeULqHZVitPM",
    "transactionDateTime": "2025-10-20T10:24:18"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |
| transactionDateTime | Timestamp | Mandatory | Waktu terbuatnya transaksi |


### 5.4 Response Body

**Contoh Response pada produk Pulsa setelah dilakukan pengecekan status transaksi**

```shell
{
    "id": 6571,
    "type": "PULSA",
    "productCode": "PUPRTEL010K",
    "transactionNumber": "INV/BIL/PUL/251020/0137QZ83HIZ",
    "partnerReferenceNo": "uQmi67EughqXhzLovTcmAeULqHZVitPM",
    "beneficiaryNumber": "081100000001",
    "farePrice": 10600.00,
    "serviceCharge": 0.00,
    "discount": 0.00,
    "totalPrice": 10600.00,
    "margin": 0.00,
    "grandTotal": 10600.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 10182,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PUL/251020/0137QZ83HIZ",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Telkomsel 10rb",
            "label": "Nominal Pulsa"
        },
        {
            "key": "beneficiaryNumber",
            "value": "081100000001",
            "label": "No. Telepon"
        },
        {
            "key": "provider",
            "value": "Telkomsel",
            "label": "Provider"
        },
        {
            "key": "farePrice",
            "value": "Rp10.600",
            "label": "Harga"
        }
    ]
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| id | Int | Mandatory | Kode unik hasil transaksi |
| transactionNumber | Int | Mandatory | Kode unik transaksi yang digenerate oleh AstraPay |
| type | String | Mandatory | Kode untuk kategori produk (Contoh: PULSA) |
| productCode | String | Mandatory | Kode untuk produk (Contoh: PUPRTEL011K) |
| serviceCharge | Int | Mandatory | Biaya layanan yang dikenakan kepada Merchant |
| totalPrice | Int | Mandatory | Nominal harga jual dari AstraPay (Rumus: *farePrice + serviceCharge - discount*) |
| margin | Int | Mandatory | Keuntungan yang didapatkan Merchant |
| farePrice | Int | Mandatory | Harga pokok sebelum penambahan biaya layanan, diskon, atau biaya tambahan lainnya |
| discount | Int | Mandatory | Potongan harga yang diberikan oleh AstraPay |
| grandTotal | Int | Mandatory | Nominal harga jual yang ditambahkan margin (Rumus: *totalPrice + margin*) |
| message | String | Optional | Pesan yang dimunculkan ketika terjadi error |
| status | String | Mandatory | Status Transaksi (SUCCESS / PENDING / VOID) |
| inquiryId | String | Mandatory | Kode unik hasil inquiry untuk transaksi |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |
| detailAdditionalData | Object | Mandatory | Data tambahan dari inquiry |
| detailAdditionalData.key | String | Optional | Kata kunci untuk penunjuk data |
| detailAdditionalData.value | String | Optional | Response data |
| detailAdditionalData.label | String | Optional | Label keterangan data |


## 6. List Response untuk Semua Produk

## 6.1 Angsuran

**Request Inquiry Angsuran ACC**

```shell
{
  "beneficiaryNumber": "0230030600184090",
  "productCode": "ACC"
}
```

**Response Inquiry Angsuran ACC**

```shell
{
    "id": 1334,
    "type": "ANGSURAN",
    "productCode": "ACC",
    "beneficiaryNumber": "0740040700159877",
    "minimumPayment": 2932000.00,
    "farePrice": 2930000.00,
    "serviceCharge": 2000.00,
    "totalPrice": 2932000.00,
    "discount": 0.00,
    "margin": 0.00,
    "grandTotal": 2932000.00,
    "message": null,
    "createdAt": "2024-08-20T13:49:36.615552",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "ACC",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "NANANG FAUZI KURNIAWAN",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "0740040700159877",
            "label": "No. Perjanjian"
        },
        {
            "key": "installmentNumber",
            "value": "6 / null bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "basicPrice",
            "value": "Rp2.930.000",
            "label": "Jumlah Angsuran"
        },
        {
            "key": "fine",
            "value": "Rp0",
            "label": "Denda"
        },
        {
            "key": "includedServiceChargeCollection",
            "value": "Rp0",
            "label": "Late Charge"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Angsuran ACC**

```shell
{
  "inquiryId": "1334",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Angsuran ACC**

```shell
{
    "id": 5038,
    "type": "ANGSURAN",
    "productCode": "ACC",
    "transactionNumber": "INV/BIL/ANG/240820/004OAJL78N7",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "0740040700159877",
    "farePrice": 2930000.00,
    "serviceCharge": 2000.00,
    "discount": 0.00,
    "totalPrice": 2932000.00,
    "margin": 0.00,
    "grandTotal": 2932000.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1334,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ANG/240820/004OAJL78N7",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "ACC",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "NANANG FAUZI KURNIAWAN",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "0740040700159877",
            "label": "No. Perjanjian"
        },
        {
            "key": "installmentNumber",
            "value": "6 / 48 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "basicPrice",
            "value": "Rp2.930.000",
            "label": "Jumlah Angsuran"
        },
        {
            "key": "fine",
            "value": "Rp0",
            "label": "Denda"
        },
        {
            "key": "includedServiceChargeCollection",
            "value": "Rp0",
            "label": "Late Charge"
        }
    ]
}
```

**Response Mendapatkan Status Angsuran ACC**

```shell
{
    "id": 5038,
    "type": "ANGSURAN",
    "productCode": "ACC",
    "transactionNumber": "INV/BIL/ANG/240820/004OAJL78N7",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "0740040700159877",
    "farePrice": 2930000.00,
    "serviceCharge": 2000.00,
    "discount": 0.00,
    "totalPrice": 2932000.00,
    "margin": 0.00,
    "grandTotal": 2932000.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1334,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ANG/240820/004OAJL78N7",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "ACC",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "NANANG FAUZI KURNIAWAN",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "0740040700159877",
            "label": "No. Perjanjian"
        },
        {
            "key": "installmentNumber",
            "value": "6 / 48 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "basicPrice",
            "value": "Rp2.930.000",
            "label": "Jumlah Angsuran"
        },
        {
            "key": "fine",
            "value": "Rp0",
            "label": "Denda"
        },
        {
            "key": "includedServiceChargeCollection",
            "value": "Rp0",
            "label": "Late Charge"
        }
    ]
}
```

**Request Inquiry Angsuran TAF**

```shell
{
  "beneficiaryNumber": "0230030600184090",
  "productCode": "TAF"
}
```

**Response Inquiry Angsuran TAF**

```shell
{
    "id": 1336,
    "type": "ANGSURAN",
    "productCode": "TAF",
    "beneficiaryNumber": "2111755774",
    "minimumPayment": 19975712.00,
    "farePrice": 19973712.00,
    "serviceCharge": 2000.00,
    "discount": 0.00,
    "totalPrice": 19975712.00,
    "margin": 0.00,
    "grandTotal": 19975712.00,
    "message": null,
    "createdAt": "2024-08-20T14:08:58.056024",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "TAF",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "VICTOR YOHANES TUMANGGOR SH",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "2111755774",
            "label": "No. Virtual Account"
        },
        {
            "key": "installmentNumber",
            "value": "29 / 60 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "farePrice",
            "value": "Rp19.973.712",
            "label": "Jumlah Angsuran"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Angsuran TAF**

```shell
{
  "inquiryId": "1336",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Angsuran TAF**

```shell
{
    "id": 5039,
    "type": "ANGSURAN",
    "productCode": "TAF",
    "transactionNumber": "INV/BIL/ANG/240820/003W44DEDS7",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "2111755774",
    "farePrice": 19973712.00,
    "serviceCharge": 2000.00,
    "discount": 0.00,
    "totalPrice": 19975712.00,
    "margin": 0.00,
    "grandTotal": 19975712.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1336,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ANG/240820/003W44DEDS7",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "TAF",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "VICTOR YOHANES TUMANGGOR SH",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "2111755774",
            "label": "No. Virtual Account"
        },
        {
            "key": "installmentNumber",
            "value": "29 / 60 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "farePrice",
            "value": "Rp19.973.712",
            "label": "Jumlah Angsuran"
        }
    ]
}
```

**Response Mendapatkan Status Angsuran TAF**

```shell
{
    "id": 5039,
    "type": "ANGSURAN",
    "productCode": "TAF",
    "transactionNumber": "INV/BIL/ANG/240820/003W44DEDS7",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "2111755774",
    "farePrice": 19973712.00,
    "serviceCharge": 2000.00,
    "discount": 0.00,
    "totalPrice": 19975712.00,
    "margin": 0.00,
    "grandTotal": 19975712.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1336,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ANG/240820/003W44DEDS7",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "TAF",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "VICTOR YOHANES TUMANGGOR SH",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "2111755774",
            "label": "No. Virtual Account"
        },
        {
            "key": "installmentNumber",
            "value": "29 / 60 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "farePrice",
            "value": "Rp19.973.712",
            "label": "Jumlah Angsuran"
        }
    ]
}
```

Produk angsuran adalah produk pembayaran tagihan dengan nominal yang fix.


| Label | Type Code |
| --- | --- |
| Angsuran | ANGSURAN |


Beberapa produk angsuran yang tersedia:


| Produk Angsuran | Description |
| --- | --- |
| Angsuran ACC | Angsuran Otomotif dari perusahaan Astra Credit Company (ACC) |
| Angsuran TAF | Angsuran Otomotif dari perusahaan Toyota Astra Finance (TAF) |
| Angsuran Maucash | Angsuran Paylater dari perusahaan Maucash |
| Angsuran Adira Finance | Angsuran Otomotif dari perusahaan Federal International Finance (FIF) |
| Angsuran Bussan Auto Finance | Angsuran Otomotif dari perusahaan Bussan Auto Finance (BAF) |
| Angsuran WOM Finance | Angsuran Otomotif dari perusahaan Wahana Ottomitra Multiartha |
| Angsuran Home Credit | Angsuran dari perusahaan Home Credit |
| Angsuran Kreditplus Finansia | Angsuran dari perusahaan Kredit Plus |
| Angsuran AEON | Angsuran dari perusahaan AEON |
| Angsuran Artha Prima Finance | Angsuran Otomotif dari perusahaan Artha Prima Finance |
| Angsuran Bima Finance | Angsuran Otomotif dari perusahaan Bima Multi Finance |
| Angsuran Clipan Finance | Angsuran Otomotif dari perusahaan Clipan Finance Indonesia |
| Angsuran Mandala Finance | Angsuran dari perusahaan Mandala Multifinance |
| Angsuran Mandiri Utama Finance | Angsuran dari perusahaan Mandiri Utama Finance |
| Angsuran Mega Auto Central Finance | Angsuran Otomotif dari perusahaan Mega Central Finance |
| Angsuran MNC Finance | Angsuran dari perusahaan MNC Finance |
| Angsuran Multindo Auto Finance | Angsuran Otomotif dari perusahaan Multindo Auto Finance |
| Angsuran NSC Finance | Angsuran Otomotif dari perusahaan Nusa Surya Ciptadana |
| Angsuran Smart Multi Finance | Angsuran Otomotif dari perusahaan Smart Multi Finance |
| Angsuran Summit OTO Finance | Angsuran Otomotif dari perusahaan Summit Oto Finance |
| Angsuran Suzuki Finance | Angsuran Otomotif dari perusahaan Suzuki Finance Indonesia |
| Angsuran True Finance | Angsuran Otomotif dari perusahaan True Finance |


## 6.2 Angsuran Dinamis

**Request Inquiry Angsuran Dinamis**

```shell
request
{
    "beneficiaryNumber": "846000494119",
    "productCode": "FIF"
}
```

**Response Inquiry Angsuran Dinamis**

```shell
{
    "id": 1342,
    "type": "ANGSURAN",
    "productCode": "FIF",
    "beneficiaryNumber": "846000494119",
    "minimumPayment": 410000.00,
    "farePrice": 3611700.00,
    "serviceCharge": 2000.00,
    "discount": 2000.00,
    "totalPrice": 3611700.00,
    "margin": 0.00,
    "grandTotal": 3611700.00,
    "message": null,
    "createdAt": "2024-08-20T14:31:45.220085",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "Biller FIF",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "YUSNIAR HEKSA SEOMADI",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "846000494119",
            "label": "No. Kontrak"
        },
        {
            "key": "dueDate",
            "value": "26 Mar 2021",
            "label": "Jatuh Tempo"
        },
        {
            "key": "installmentNumber",
            "value": "22 / 24 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "platform",
            "value": "K",
            "label": "Platform"
        },
        {
            "key": "installmentAmount",
            "value": "Rp400.000",
            "label": "Tagihan Angsuran"
        },
        {
            "key": "outstandingPenalty",
            "value": "Rp3.148.900",
            "label": "Outstanding Denda"
        },
        {
            "key": "outstandingCollectionFee",
            "value": "Rp62.800",
            "label": "Outstanding Coll Fee"
        },
        {
            "key": "fine",
            "value": "0",
            "label": "Denda Terbayar"
        },
        {
            "key": "includedServiceChargeCollection",
            "value": "0",
            "label": "Coll Fee Terbayar"
        },
        {
            "key": "totalPrice",
            "value": "Rp3.611.700",
            "label": "Total Kewajiban"
        },
        {
            "key": "minimumPayment",
            "value": "Rp410.000",
            "label": "Minimal Pembayaran"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "DYNAMIC"
}
```

**Request Transaksi Angsuran Dinamis**

```shell
{
    "inquiryId": "1342",
    "amount": "411000",
    "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Angsuran Dinamis**

```shell
{
    "id": 5040,
    "type": "ANGSURAN",
    "productCode": "FIF",
    "transactionNumber": "INV/BIL/ANG/240820/002STSYT5QL",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "846000494119",
    "farePrice": 411000.00,
    "serviceCharge": 2000.00,
    "discount": 2000.00,
    "totalPrice": 411000.00,
    "margin": 0.00,
    "grandTotal": 411000.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1342,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ANG/240820/002STSYT5QL",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Biller FIF",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "YUSNIAR HEKSA SEOMADI",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "846000494119",
            "label": "No. Kontrak"
        },
        {
            "key": "dueDate",
            "value": "26 Mar 2021",
            "label": "Jatuh Tempo"
        },
        {
            "key": "installmentNumber",
            "value": "22 / 24 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "platform",
            "value": "K",
            "label": "Platform"
        },
        {
            "key": "installmentAmount",
            "value": "Rp400.000",
            "label": "Tagihan Angsuran"
        },
        {
            "key": "fine",
            "value": "Rp10.000",
            "label": "Denda Terbayar"
        },
        {
            "key": "includedServiceChargeCollection",
            "value": "Rp1.000",
            "label": "Coll Fee Terbayar"
        }
    ]
}
```

**Response Mendapatkan Status Angsuran Dinamis**

```shell
{
    "id": 5040,
    "type": "ANGSURAN",
    "productCode": "FIF",
    "transactionNumber": "INV/BIL/ANG/240820/002STSYT5QL",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "846000494119",
    "farePrice": 411000.00,
    "serviceCharge": 2000.00,
    "discount": 2000.00,
    "totalPrice": 411000.00,
    "margin": 0.00,
    "grandTotal": 411000.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1342,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ANG/240820/002STSYT5QL",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Biller FIF",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryName",
            "value": "YUSNIAR HEKSA SEOMADI",
            "label": "Nama"
        },
        {
            "key": "beneficiaryNumber",
            "value": "846000494119",
            "label": "No. Kontrak"
        },
        {
            "key": "dueDate",
            "value": "26 Mar 2021",
            "label": "Jatuh Tempo"
        },
        {
            "key": "installmentNumber",
            "value": "22 / 24 bulan",
            "label": "Angsuran Ke"
        },
        {
            "key": "platform",
            "value": "K",
            "label": "Platform"
        },
        {
            "key": "installmentAmount",
            "value": "Rp400.000",
            "label": "Tagihan Angsuran"
        },
        {
            "key": "fine",
            "value": "Rp10.000",
            "label": "Denda Terbayar"
        },
        {
            "key": "includedServiceChargeCollection",
            "value": "Rp1.000",
            "label": "Coll Fee Terbayar"
        }
    ]
}
```

Produk angsuran dinamis adalah produk pembayaran tagihan dengan nominal yang dapat ditentukan oleh User dengan minimum tertentu. Produk angsuran dinamis tersedia pada Angsuran FIF (Federal International Finance) untuk angsuran otomotif.


| Label | Type Code |
| --- | --- |
| Angsuran | ANGSURAN |


## 6.3 Pulsa

**Request Inquiry Pulsa**

```shell
{
  "beneficiaryNumber": "082100001111", 
  "productCode": "PUPRTEL025K"
}
```

**Response Inquiry Pulsa**

```shell
{
    "id": 1349,
    "type": "PULSA",
    "productCode": "PUPRTEL025K",
    "beneficiaryNumber": "082100001111",
    "minimumPayment": 25600.00,
    "farePrice": 25500.00,
    "serviceCharge": 100.00,
    "discount": 0.00,
    "totalPrice": 25600.00,
    "margin": 0.00,
    "grandTotal": 25600.00,
    "message": null,
    "createdAt": "2024-08-20T14:53:41.576966",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "Telkomsel 25rb",
            "label": "Nominal Pulsa"
        },
        {
            "key": "beneficiaryNumber",
            "value": "082100001111",
            "label": "No. Telepon"
        },
        {
            "key": "provider",
            "value": "Telkomsel",
            "label": "Provider"
        },
        {
            "key": "farePrice",
            "value": "Rp25.500",
            "label": "Harga"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Pulsa**

```shell
{
  "inquiryId": "1349",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Pulsa**

```shell
{
    "id": 5042,
    "type": "PULSA",
    "productCode": "PUPRTEL025K",
    "transactionNumber": "INV/BIL/PUL/240820/006089BGONY",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "082100001111",
    "farePrice": 25500.00,
    "serviceCharge": 100.00,
    "discount": 0.00,
    "totalPrice": 25600.00,
    "margin": 0.00,
    "grandTotal": 25600.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 1349,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PUL/240820/006089BGONY",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Telkomsel 25rb",
            "label": "Nominal Pulsa"
        },
        {
            "key": "beneficiaryNumber",
            "value": "082100001111",
            "label": "No. Telepon"
        },
        {
            "key": "provider",
            "value": "Telkomsel",
            "label": "Provider"
        },
        {
            "key": "farePrice",
            "value": "Rp25.500",
            "label": "Harga"
        }
    ]
}
```

**Response Mendapatkan Status Pulsa**

```shell
{
    "id": 5042,
    "type": "PULSA",
    "productCode": "PUPRTEL025K",
    "transactionNumber": "INV/BIL/PUL/240820/006089BGONY",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "082100001111",
    "farePrice": 25500.00,
    "serviceCharge": 100.00,
    "discount": 0.00,
    "totalPrice": 25600.00,
    "margin": 0.00,
    "grandTotal": 25600.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1349,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PUL/240820/006089BGONY",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Telkomsel 25rb",
            "label": "Nominal Pulsa"
        },
        {
            "key": "beneficiaryNumber",
            "value": "082100001111",
            "label": "No. Telepon"
        },
        {
            "key": "provider",
            "value": "Telkomsel",
            "label": "Provider"
        },
        {
            "key": "farePrice",
            "value": "Rp25.500",
            "label": "Harga"
        }
    ]
}
```

Produk pulsa yang tersedia adalah Axis, Indosat, Smartfren, Telkomsel, Tri, dan XL.


| Label | Type Code |
| --- | --- |
| Pulsa | PULSA |


## 6.4 Pulsa Pascabayar

**Request Inquiry Pulsa Pascabayar**

```shell
{
  "beneficiaryNumber": "082295095786",
  "productCode": "PUPPTELHALO"
  }
```

**Response Inquiry Pulsa Pascabayar**

```shell
{
    "id": 2218,
    "type": "PASCABAYAR",
    "productCode": "PUPPTELHALO",
    "beneficiaryNumber": "082295095786",
    "minimumPayment": 104000.00,
    "farePrice": 100000.00,
    "serviceCharge": 4000.00,
    "discount": 0.00,
    "totalPrice": 104000.00,
    "margin": 0.00,
    "grandTotal": 104000.00,
    "message": null,
    "createdAt": "2024-08-20T15:59:44.824619",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "Telkomsel Halo",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "082295095786",
            "label": "No. Telepon"
        },
        {
            "key": "beneficiaryName",
            "value": "DUMMY HALO-01",
            "label": "Nama"
        },
        {
            "key": "basicPrice",
            "value": "Rp100.000",
            "label": "Jumlah Tagihan"
        }
    ],
    "status": "SUCCESS",
    "paymentType": null
}
```

**Request Transaksi Pulsa Pascabayar**

```shell
{
  "inquiryId": "1408",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Pulsa Pascabayar**

```shell
{
    "id": 2066,
    "type": "PASCABAYAR",
    "productCode": "PUPPTELHALO",
    "transactionNumber": "INV/BIL/PUL/240820/0055HT7P61N",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "082295095786",
    "farePrice": 100000.00,
    "serviceCharge": 4000.00,
    "discount": 0.00,
    "totalPrice": 104000.00,
    "margin": 0.00,
    "grandTotal": 104000.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 2218,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PUL/240820/0055HT7P61N",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Telkomsel Halo",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "082295095786",
            "label": "No. Telepon"
        },
        {
            "key": "beneficiaryName",
            "value": "DUMMY HALO-01",
            "label": "Nama"
        },
        {
            "key": "basicPrice",
            "value": "Rp100.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

**Response Mendapatkan Status Pulsa Pascabayar**

```shell
{
    "id": 2066,
    "type": "PASCABAYAR",
    "productCode": "PUPPTELHALO",
    "transactionNumber": "INV/BIL/PUL/240820/0055HT7P61N",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "082295095786",
    "farePrice": 100000.00,
    "serviceCharge": 4000.00,
    "discount": 0.00,
    "totalPrice": 104000.00,
    "margin": 0.00,
    "grandTotal": 104000.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 2218,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PUL/240820/0055HT7P61N",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Telkomsel Halo",
            "label": "Nama Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "082295095786",
            "label": "No. Telepon"
        },
        {
            "key": "beneficiaryName",
            "value": "DUMMY HALO-01",
            "label": "Nama"
        },
        {
            "key": "basicPrice",
            "value": "Rp100.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

Produk pulsa pascabayar yang tersedia adalah Axis, Indosat, Smartfren, Telkomsel, Tri, dan XL.


| Label | Type Code |
| --- | --- |
| Pascabayar | PASCABAYAR |


## 6.5 Paket Data

**Request Inquiry Paket Data**

```shell
{
  "beneficiaryNumber": "08129550246",
  "productCode": "TD100"
}
```

**Response Inquiry Paket Data**

```shell
{
    "id": 1369,
    "type": "PAKET_DATA",
    "productCode": "TD100",
    "beneficiaryNumber": "08129550246",
    "minimumPayment": 100000.00,
    "farePrice": 100000.00,
    "serviceCharge": 0.00,
    "discount": 0.00,
    "totalPrice": 100000.00,
    "margin": 0.00,
    "grandTotal": 100000.00,
    "message": null,
    "createdAt": "2024-08-20T16:17:36.161625",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "12 GB Kuota Utama + 2 GB OMG Masa Aktif 30 Hari",
            "label": "Nama Paket"
        },
        {
            "key": "beneficiaryNumber",
            "value": "08129550246",
            "label": "No. Telepon"
        },
        {
            "key": "provider",
            "value": "Telkomsel",
            "label": "Provider"
        },
        {
            "key": "farePrice",
            "value": "Rp100.000",
            "label": "Harga"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Paket Data**

```shell
{
  "inquiryId": "1369",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Paket Data**

```shell
{
    "id": 5044,
    "type": "PAKET_DATA",
    "productCode": "TD100",
    "transactionNumber": "INV/BIL/PAD/240820/006QTMRTTYZ",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "08129550246",
    "farePrice": 100000.00,
    "serviceCharge": 0.00,
    "discount": 0.00,
    "totalPrice": 100000.00,
    "margin": 0.00,
    "grandTotal": 100000.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 1369,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PAD/240820/006QTMRTTYZ",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "12 GB Kuota Utama + 2 GB OMG Masa Aktif 30 Hari",
            "label": "Nama Paket"
        },
        {
            "key": "beneficiaryNumber",
            "value": "08129550246",
            "label": "No. Telepon"
        },
        {
            "key": "provider",
            "value": "Telkomsel",
            "label": "Provider"
        },
        {
            "key": "farePrice",
            "value": "Rp100.000",
            "label": "Harga"
        }
    ]
}
```

**Response Mendapatkan Status Paket Data**

```shell
{
    "id": 5044,
    "type": "PAKET_DATA",
    "productCode": "TD100",
    "transactionNumber": "INV/BIL/PAD/240820/006QTMRTTYZ",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "08129550246",
    "farePrice": 100000.00,
    "serviceCharge": 0.00,
    "discount": 0.00,
    "totalPrice": 100000.00,
    "margin": 0.00,
    "grandTotal": 100000.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1369,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PAD/240820/006QTMRTTYZ",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "12 GB Kuota Utama + 2 GB OMG Masa Aktif 30 Hari",
            "label": "Nama Paket"
        },
        {
            "key": "beneficiaryNumber",
            "value": "08129550246",
            "label": "No. Telepon"
        },
        {
            "key": "provider",
            "value": "Telkomsel",
            "label": "Provider"
        },
        {
            "key": "farePrice",
            "value": "Rp100.000",
            "label": "Harga"
        }
    ]
}
```

Produk paket data yang tersedia adalah Axis, Indosat, Smartfren, Telkomsel, Tri, dan XL.


| Label | Type Code |
| --- | --- |
| Paket Data | PAKET_DATA |


## 6.6 Token Listrik

**Request Inquiry Token Listrik**

```shell
{
  "beneficiaryNumber": "45017031068",
  "productCode": "PLPRPLN020K"
}
```

**Response Inquiry Token Listrik**

```shell
{
    "id": 2226,
    "type": "TOKEN_LISTRIK",
    "productCode": "PLPRPLN020K",
    "beneficiaryNumber": "45017031068",
    "minimumPayment": 20150.00,
    "farePrice": 20000.00,
    "serviceCharge": 150.00,
    "discount": 0.00,
    "totalPrice": 20150.00,
    "margin": 0.00,
    "grandTotal": 20150.00,
    "message": null,
    "createdAt": "2024-08-20T16:28:25.542082",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "Token 20.000",
            "label": "Nominal"
        },
        {
            "key": "beneficiaryNumber",
            "value": "45017031068",
            "label": "No. Meter"
        },
        {
            "key": "beneficiaryName",
            "value": "BENI INDRA",
            "label": "Nama"
        },
        {
            "key": "farePower",
            "value": "R1/2200",
            "label": "Tarif / Daya"
        },
        {
            "key": "basicPrice",
            "value": "Rp20.000",
            "label": "Harga"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Token Listrik**

```shell
{
   "inquiryId": "2226",
   "partnerReferenceNo": "1A23F45"
   }
```

**Response Transaksi Token Listrik**

```shell
{
    "id": 2067,
    "type": "TOKEN_LISTRIK",
    "productCode": "PLPRPLN020K",
    "transactionNumber": "INV/BIL/PLN/240820/006VTWS6CNR",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "45017031068",
    "farePrice": 20000.00,
    "serviceCharge": 150.00,
    "discount": 0.00,
    "totalPrice": 20150.00,
    "margin": 0.00,
    "grandTotal": 20150.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 2226,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PLN/240820/006VTWS6CNR",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Token 20.000",
            "label": "Nominal"
        },
        {
            "key": "beneficiaryNumber",
            "value": "45017031068",
            "label": "No. Meter"
        },
        {
            "key": "beneficiaryName",
            "value": "BENI INDRA",
            "label": "Nama"
        },
        {
            "key": "farePower",
            "value": "R1/2200",
            "label": "Tarif / Daya"
        },
        {
            "key": "basicPrice",
            "value": "Rp20.000",
            "label": "Harga"
        }
    ]
}
```

**Response Mendapatkan Status Token Listrik**

```shell
{
    "id": 2067,
    "type": "TOKEN_LISTRIK",
    "productCode": "PLPRPLN020K",
    "transactionNumber": "INV/BIL/PLN/240820/006VTWS6CNR",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "45017031068",
    "farePrice": 20000.00,
    "serviceCharge": 150.00,
    "discount": 0.00,
    "totalPrice": 20150.00,
    "margin": 0.00,
    "grandTotal": 20150.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 2226,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PLN/240820/006VTWS6CNR",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Token 20.000",
            "label": "Nominal"
        },
        {
            "key": "beneficiaryNumber",
            "value": "45017031068",
            "label": "No. Meter"
        },
        {
            "key": "beneficiaryName",
            "value": "BENI INDRA",
            "label": "Nama"
        },
        {
            "key": "farePower",
            "value": "R1/2200",
            "label": "Tarif / Daya"
        },
        {
           "key": "totalUsage",
           "value": "574,70 kWh",
           "label": "Jumlah kWh"
        },
        {
          "key": "stroomToken",
          "value": "0632 1234 5678 1234",
          "label": "Stroom / Token"
        },
        {
            "key": "basicPrice",
            "value": "Rp20.000",
            "label": "Harga"
        }
    ]
}
```

Token Listrik adalah produk prabayar listrik PLN.


| Label | Type Code |
| --- | --- |
| Token Listrik | TOKEN_LISTRIK |


## 6.7 Tagihan Listrik

**Request Inquiry Tagihan Listrik**

```shell
{
  "beneficiaryNumber": "516070377775",
  "productCode": "PLPOPLN0000"
}
```

**Response Inquiry Tagihan Listrik**

```shell
{
    "id": 2228,
    "type": "TAGIHAN_LISTRIK",
    "productCode": "PLPOPLN0000",
    "beneficiaryNumber": "516070377775",
    "minimumPayment": 25720.00,
    "farePrice": 22220.00,
    "serviceCharge": 3500.00,
    "discount": 0.00,
    "totalPrice": 25720.00,
    "margin": 0.00,
    "grandTotal": 25720.00,
    "message": null,
    "createdAt": "2024-08-20T16:37:55.857021",
    "detailAdditionalData": [
        {
            "key": "itemName",
            "value": "Tagihan Listrik PLN",
            "label": "Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "516070377775",
            "label": "ID Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "MAKIMA",
            "label": "Nama"
        },
        {
            "key": "farePower",
            "value": "R1/000000450",
            "label": "Tarif / Daya"
        },
        {
            "key": "periode",
            "value": "Februari 2016",
            "label": "Periode Tagihan"
        },
        {
            "key": "totalUsage",
            "value": "5400 kWh",
            "label": "Jumlah Pemakaian"
        },
        {
            "key": "basicPrice",
            "value": "Rp22.220",
            "label": "Jumlah Tagihan"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Tagihan Listrik**

```shell
{
  "inquiryId": "2228",
  "partnerReferenceNo": "1A23F45"
  }
```

**Response Transaksi Tagihan Listrik**

```shell
{
    "id": 2068,
    "type": "TAGIHAN_LISTRIK",
    "productCode": "PLPOPLN0000",
    "transactionNumber": "INV/BIL/PLN/240820/005FUVIJOM1",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "516070377775",
    "farePrice": 22220.00,
    "serviceCharge": 3500.00,
    "discount": 0.00,
    "totalPrice": 25720.00,
    "margin": 0.00,
    "grandTotal": 25720.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 2228,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PLN/240820/005FUVIJOM1",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Tagihan Listrik PLN",
            "label": "Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "516070377775",
            "label": "ID Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "MAKIMA",
            "label": "Nama"
        },
        {
            "key": "farePower",
            "value": "R1/000000450",
            "label": "Tarif / Daya"
        },
        {
            "key": "standMeter",
            "value": "470300-475700",
            "label": "Stand Meter"
        },
        {
            "key": "periode",
            "value": "Februari 2016",
            "label": "Periode Tagihan"
        },
        {
            "key": "totalUsage",
            "value": "5400 kWh",
            "label": "Jumlah Pemakaian"
        },
        {
            "key": "basicPrice",
            "value": "Rp22.220",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

**Response Mendapatkan Status Tagihan Listrik**

```shell
{
    "id": 2068,
    "type": "TAGIHAN_LISTRIK",
    "productCode": "PLPOPLN0000",
    "transactionNumber": "INV/BIL/PLN/240820/005FUVIJOM1",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "516070377775",
    "farePrice": 22220.00,
    "serviceCharge": 3500.00,
    "discount": 0.00,
    "totalPrice": 25720.00,
    "margin": 0.00,
    "grandTotal": 25720.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 2228,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PLN/240820/005FUVIJOM1",
            "label": "No. Transaksi"
        },
        {
            "key": "itemName",
            "value": "Tagihan Listrik PLN",
            "label": "Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "516070377775",
            "label": "ID Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "MAKIMA",
            "label": "Nama"
        },
        {
            "key": "farePower",
            "value": "R1/000000450",
            "label": "Tarif / Daya"
        },
        {
            "key": "standMeter",
            "value": "470300-475700",
            "label": "Stand Meter"
        },
        {
            "key": "periode",
            "value": "Februari 2016",
            "label": "Periode Tagihan"
        },
        {
            "key": "totalUsage",
            "value": "5400 kWh",
            "label": "Jumlah Pemakaian"
        },
        {
            "key": "basicPrice",
            "value": "Rp22.220",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

Tagihan listrik adalah produk pascabayar Listrik PLN.


| Label | Type Code |
| --- | --- |
| Tagihan Listrik | TAGIHAN_LISTRIK |


## 6.8 Tagihan Air

**Request Inquiry Tagihan Air**

```shell
{
  "beneficiaryNumber": "1234567890",
  "productCode": "PDPOPDA0000"
}
```

**Response Inquiry Tagihan Air**

```shell
{
    "id": 1473,
    "type": "TAGIHAN_AIR",
    "productCode": "PDPOPDA0000",
    "beneficiaryNumber": "1234567890",
    "farePrice": 138479.00,
    "minimumPayment": 140979.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 140979.00,
    "margin": 0.00,
    "grandTotal": 140979.00,
    "message": null,
    "createdAt": "2025-03-10T15:56:57.781951",
    "detailAdditionalData": [
        {
            "key": "productName",
            "value": "PDAM PALIJA JAKARTA",
            "label": "Lokasi"
        },
        {
            "key": "beneficiaryNumber",
            "value": "1234567890",
            "label": "No. Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "LIM SUI SIAN",
            "label": "Nama"
        },
        {
            "key": "periode",
            "value": "September 2018",
            "label": "Periode Tagihan"
        },
        {
            "key": "basicPrice",
            "value": "Rp138.479",
            "label": "Jumlah Tagihan"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Tagihan Air**

```shell
{
  "inquiryId": "1473",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Tagihan Air**

```shell
{
            "id": 1761,
            "transactionNumber": "INV/BIL/PDA/240314/006BWC1T0KO",
            "type": "TAGIHAN_AIR",
            "productCode": "PDPOPDA0000",
            "farePrice": 138479.00,
            "serviceCharge": 2500.00,
            "discount": 0.00,
            "beneficiaryNumber": "1234567890",
            "totalPrice": 140979.00,
            "margin": 0.00,
            "grandTotal": 140979.00,
            "message": null,
            "status": "PENDING",
            "inquiryId": 1473,
            "partnerReferenceNo": "1A23F45",
            "detailAdditionalData": [
                {
                    "key": "transactionNumber",
                    "value": "INV/BIL/PDA/240314/006BWC1T0KO",
                    "label": "No. Transaksi"
                },
                {
                    "key": "productName",
                    "value": "PAM PALYJA",
                    "label": "Lokasi"
                },
                {
                    "key": "beneficiaryNumber",
                    "value": "1234567890",
                    "label": "No. Pelanggan"
                },
                {
                    "key": "beneficiaryName",
                    "value": "LIM SUI SIAN",
                    "label": "Nama"
                },
                {
                    "key": "periode",
                    "value": "SEP18",
                    "label": "Periode Tagihan"
                },
                {
                    "key": "basicPrice",
                    "value": "Rp138.479",
                    "label": "Jumlah Tagihan"
                }
            ]
        }
```

**Response Mendapatkan Status Tagihan Air**

```shell
{
    "id": 1761,
    "transactionNumber": "INV/BIL/PDA/240314/006BWC1T0KO",
    "type": "TAGIHAN_AIR",
    "productCode": "PDPOPDA0000",
    "farePrice": 138479.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "beneficiaryNumber": "1234567890",
    "totalPrice": 140979.00,
    "margin": 0.00,
    "grandTotal": 140979.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1473,
    "partnerRefNumber": "cropdrdevtest1",
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PDA/240308/006GN5Z03YC",
            "label": "No. Transaksi"
        },
        {
            "key": "productName",
            "value": "PAM PALYJA",
            "label": "Lokasi"
        },
        {
            "key": "beneficiaryNumber",
            "value": "1234567890",
            "label": "No. Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "LIM SUI SIAN",
            "label": "Nama"
        },
        {
            "key": "periode",
            "value": "SEP18",
            "label": "Periode Tagihan"
        },
        {
            "key": "basicPrice",
            "value": "Rp138.479",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

Tagihan air adalah produk pascabayar yang bisa disebut sebagai tagihan PDAM.


| Label | Type Code |
| --- | --- |
| Tagihan Air | TAGIHAN_AIR |


## 6.9 Internet dan TV Kabel

**Request Inquiry Internet dan TV Kabel**

```shell
{
  "beneficiaryNumber": "1234567890",
  "productCode": "INPOTEL0000"
}
```

**Response Inquiry Internet dan TV Kabel**

```shell
{
    "id": 1472,
    "type": "INTERNET_TV_KABEL",
    "productCode": "INPOTEL0000",
    "beneficiaryNumber": "1234567890",
    "farePrice": 290500.00,
    "minimumPayment": 293000.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 293000.00,
    "margin": 0.00,
    "grandTotal": 293000.00,
    "message": null,
    "createdAt": "2025-03-10T16:00:29.413596",
    "detailAdditionalData": [
        {
            "key": "productName",
            "value": "IndiHome",
            "label": "Jenis Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "1234567890",
            "label": "Nomor Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": " KOMALA DWI HAPSARI           ",
            "label": "Nama Pelanggan"
        },
        {
            "key": "period",
            "value": "Maret 2025",
            "label": "Periode Bayar"
        },
        {
            "key": "basicPrice",
            "value": "Rp290.500",
            "label": "Jumlah Tagihan"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Internet dan TV Kabel**

```shell
{
  "inquiryId": "1472",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Internet dan TV Kabel**

```shell
{
          "id": 1764,
          "transactionNumber": "INV/BIL/INT/240314/006O0P2156R",
          "type": "INTERNET_TV_KABEL",
          "productCode": "INPOTEL0000",
          "beneficiaryNumber": "1234567890",
          "farePrice": 290500.00,
          "serviceCharge": 2500.00,
          "discount": 0.00,
          "totalPrice": 293000.00,
          "margin": 0.00,
          "grandTotal": 293000.00,
          "message": null,
          "status": "PENDING",
          "inquiryId": 1472,
          "partnerReferenceNo": "1A23F45",
          "detailAdditionalData": [
              {
                  "key": "transactionNumber",
                  "value": "INV/BIL/INT/240314/006O0P2156R",
                  "label": "No. Transaksi"
              },
              {
                  "key": "productName",
                  "value": "IndiHome",
                  "label": "Jenis Layanan"
              },
              {
                  "key": "beneficiaryNumber",
                  "value": "1234567890",
                  "label": "Nomor Pelanggan"
              },
              {
                  "key": "beneficiaryName",
                  "value": "KOMALA DWI HAPSARI",
                  "label": "Nama Pelanggan"
              },
              {
                  "key": "period",
                  "value": "MARET",
                  "label": "Periode Bayar"
              },
              {
                  "key": "basicPrice",
                  "value": "Rp290.500",
                  "label": "Jumlah Tagihan"
              }
          ]
      }
```

**Response Mendapatkan Status Internet dan TV Kabel**

```shell
{
    "id": 1764,
    "transactionNumber": "INV/BIL/INT/240314/006O0P2156R",
    "type": "INTERNET_TV_KABEL",
    "productCode": "INPOTEL0000",
    "beneficiaryNumber": "1234567890",
    "farePrice": 290500.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 293000.00,
    "margin": 0.00,
    "grandTotal": 293000.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1472,
    "partnerReferenceNo": "1A23F45",
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/INT/240314/006O0P2156R",
            "label": "No. Transaksi"
        },
        {
            "key": "productName",
            "value": "IndiHome",
            "label": "Jenis Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "1234567890",
            "label": "Nomor Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": " KOMALA DWI HAPSARI           ",
            "label": "Nama Pelanggan"
        },
        {
            "key": "period",
            "value": "MARET",
            "label": "Periode Bayar"
        },
        {
            "key": "basicPrice",
            "value": "Rp290.500",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

Internet dan TV Kabel adalah produk yang berjenis pascabayar.


| Label | Type Code |
| --- | --- |
| Internet & TV Kabel | INTERNET_TV_KABEL |


## 6.10 Telkom

**Request Inquiry Telkom**

```shell
{
  "beneficiaryNumber": "2222222222",
  "productCode": "TEPOTEL0000"
}
```

**Response Inquiry Telkom**

```shell
{
    "id": 1471,
    "type": "TELKOM",
    "productCode": "TEPOTEL0000",
    "beneficiaryNumber": "2222222222",
    "farePrice": 86780.00,
    "minimumPayment": 89280.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 89280.00,
    "margin": 0.00,
    "grandTotal": 89280.00,
    "message": null,
    "createdAt": "2025-03-10T16:05:57.781951",
    "detailAdditionalData": [
        {
            "key": "productName",
            "value": "TELKOM",
            "label": "Jenis Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "2222222222",
            "label": "No. Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "DIDIN MARZUKI",
            "label": "Nama Pelanggan"
        },
        {
            "key": "period",
            "value": "Maret - April 2024",
            "label": "Periode Bayar"
        },
        {
            "key": "totalPeriod",
            "value": "2 Bulan",
            "label": "Jumlah Periode Bayar"
        },
        {
            "key": "basicPriceFirst",
            "value": "Rp50.920",
            "label": "Tagihan Bulan 1"
        },
        {
            "key": "basicPriceSecond",
            "value": "Rp35.860",
            "label": "Tagihan Bulan 2"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Telkom**

```shell
{
  "inquiryId": "14721",
  "partnerReferenceNo": "1A23F45"
}
```

**Response Transaksi Telkom**

```shell
{
      "id": 1763,
      "transactionNumber": "INV/BIL/TEL/240314/006QP7YH0BE",
      "type": "TELKOM",
      "productCode": "TEPOTEL0000",
      "beneficiaryNumber": "2222222222",
      "farePrice": 86780.00,
      "serviceCharge": 0.00,
      "discount": 0.00,
      "totalPrice": 89280.00,
      "margin": 0.00,
      "grandTotal": 89280.00,
      "message": null,
      "status": "PENDING",
      "inquiryId": 1471,
      "partnerReferenceNo": "1A23F45",
      "detailAdditionalData": [
          {
              "key": "transactionNumber",
              "value": "INV/BIL/TEL/240314/006QP7YH0BE",
              "label": "No. Transaksi"
          },
          {
              "key": "productName",
              "value": "TELKOM",
              "label": "Jenis Layanan"
          },
          {
              "key": "beneficiaryNumber",
              "value": "2222222222",
              "label": "No. Pelanggan"
          },
          {
              "key": "beneficiaryName",
              "value": " DIDIN MARZUKI",
              "label": "Nama Pelanggan"
          },
          {
              "key": "period",
              "value": "MARET, APRIL",
              "label": "Periode Bayar"
          },
          {
              "key": "totalPeriod",
              "value": "2 Bulan",
              "label": "Jumlah Periode Bayar"
          },
          {
              "key": "basicPriceFirst",
              "value": "Rp50.920",
              "label": "Tagihan Bulan 1"
          },
          {
              "key": "basicPriceSecond",
              "value": "Rp35.860",
              "label": "Tagihan Bulan 2"
          }
      ]
  }
```

**Response Mendapatkan Status Telkom**

```shell
{
    "id": 1763,
    "transactionNumber": "INV/BIL/TEL/240314/006QP7YH0BE",
    "type": "TELKOM",
    "productCode": "TEPOTEL0000",
    "beneficiaryNumber": "2222222222",
    "farePrice": 86780.00,
    "serviceCharge": 0.00,
    "discount": 0.00,
    "totalPrice": 89280.00,
    "margin": 0.00,
    "grandTotal": 89280.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1471,
    "partnerReferenceNo": "1A23F45",
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/TEL/240314/006QP7YH0BE",
            "label": "No. Transaksi"
        },
        {
            "key": "productName",
            "value": "TELKOM",
            "label": "Jenis Layanan"
        },
        {
            "key": "beneficiaryNumber",
            "value": "2222222222",
            "label": "No. Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "DIDIN MARZUKI",
            "label": "Nama Pelanggan"
        },
        {
            "key": "period",
            "value": "MARET, APRIL",
            "label": "Periode Bayar"
        },
        {
            "key": "totalPeriod",
            "value": "2 Bulan",
            "label": "Jumlah Periode Bayar"
        },
        {
            "key": "basicPriceFirst",
            "value": "Rp50.920",
            "label": "Tagihan Bulan 1"
        },
        {
            "key": "basicPriceSecond",
            "value": "Rp35.860",
            "label": "Tagihan Bulan 2"
        }
    ]
}
```

Tagihan telkom adalah produk pascabayar untuk pembayaran telkom dan/atau Indihome.


| Label | Type Code |
| --- | --- |
| Telkom | TELKOM |


## 6.11 BPJS Kesehatan

**Request Inquiry BPJS Kesehatan**

```shell
{
  "beneficiaryNumber": "8888802883963273",
  "productCode": "ASPOBPJ0000",
  "periodStart": "2024-03-07",
  "periodEnd": "2024-03-07"
}
```

**Response Inquiry BPJS Kesehatan**

```shell
{
    "id": 1422,
    "type": "BPJS_KESEHATAN",
    "productCode": "ASPOBPJ0000",
    "beneficiaryNumber": "8888802883963273",
    "farePrice": 1750000.00,
    "minimumPayment": 1752500.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 1752500.00,
    "margin": 0.00,
    "grandTotal": 1752500.00,
    "message": null,
    "createdAt": "2025-03-10T16:19:50.093219",
    "detailAdditionalData": [
        {
            "key": "transactionType",
            "value": "BPJS Kesehatan",
            "label": "Jenis Pembayaran"
        },
        {
            "key": "beneficiaryNumber",
            "value": "8888802883963273",
            "label": "No. BPJS Kesehatan"
        },
        {
            "key": "beneficiaryName",
            "value": "TUMIRAH,DARSO SEMITO",
            "label": "Nama Pemilik"
        },
        {
            "key": "totalMembers",
            "value": "2 orang",
            "label": "Anggota Keluarga"
        },
        {
            "key": "totalPeriods",
            "value": "1 bulan",
            "label": "Periode Bayar"
        },
        {
            "key": "basicPrice",
            "value": "Rp1.750.000",
            "label": "Jumlah Tagihan"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi BPJS Kesehatan**

```shell
{
  "inquiryId": "1422",
  "partnerReferenceNo": "1A23F45",
}
```

**Response Transaksi BPJS Kesehatan**

```shell
{
      "id": 1762,
      "transactionNumber": "INV/BIL/ASU/240314/008OORD2ATP",
      "type": "BPJS_KESEHATAN",
      "productCode": "ASPOBPJ0000",
      "beneficiaryNumber": "8888802883963273",
      "farePrice": 1750000.00,
      "serviceCharge": 2500.00,
      "discount": 0.00,
      "totalPrice": 1752500.00,
      "margin": 0.00,
      "grandTotal": 1752500.00,
      "message": null,
      "status": "PENDING",
      "inquiryId": 1422,
      "partnerReferenceNo": "1A23F45",
      "detailAdditionalData": [
          {
              "key": "transactionNumber",
              "value": "INV/BIL/ASU/240314/008OORD2ATP",
              "label": "No. Transaksi"
          },
          {
              "key": "referenceNumberProvider",
              "value": "8BC3CC22FE669563",
              "label": "No. Referensi"
          },
          {
              "key": "transactionType",
              "value": "BPJS Kesehatan",
              "label": "Jenis Pembayaran"
          },
          {
              "key": "beneficiaryNumber",
              "value": "8888802883963273",
              "label": "No. BPJS Kesehatan"
          },
          {
              "key": "beneficiaryName",
              "value": "Jarister Edwins Silalahi",
              "label": "Nama Pemilik"
          },
          {
              "key": "totalMembers",
              "value": "2 orang",
              "label": "Anggota Keluarga"
          },
          {
              "key": "totalPeriods",
              "value": "1 bulan",
              "label": "Periode Bayar"
          },
          {
              "key": "basicPrice",
              "value": "Rp1.750.000",
              "label": "Jumlah Tagihan"
          }
      ]
  }
```

**Response Mendapatkan Status BPJS Kesehatan**

```shell
{
    "id": 1762,
    "transactionNumber": "INV/BIL/ASU/240314/008OORD2ATP",
    "type": "BPJS_KESEHATAN",
    "productCode": "ASPOBPJ0000",
    "beneficiaryNumber": "8888802883963273",
    "farePrice": 1750000.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 1752500.00,
    "margin": 0.00,
    "grandTotal": 1752500.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1422,
    "partnerReferenceNo": "1A23F45",
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/ASU/240314/008OORD2ATP",
            "label": "No. Transaksi"
        },
        {
            "key": "referenceNumberProvider",
            "value": "8BC3CC22FE669563",
            "label": "No. Referensi"
        },
        {
            "key": "transactionType",
            "value": "BPJS Kesehatan",
            "label": "Jenis Pembayaran"
        },
        {
            "key": "beneficiaryNumber",
            "value": "8888802883963273",
            "label": "No. BPJS Kesehatan"
        },
        {
            "key": "beneficiaryName",
            "value": "Jarister Edwins Silalahi",
            "label": "Nama Pemilik"
        },
        {
            "key": "totalMembers",
            "value": "2 orang",
            "label": "Anggota Keluarga"
        },
        {
            "key": "totalPeriods",
            "value": "1 bulan",
            "label": "Periode Bayar"
        },
        {
            "key": "basicPrice",
            "value": "Rp1.750.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

Produk BPJS Kesehatan adalah produk pascabayar pembayaran asuransi kesehatan pemerintah.


| Label | Type Code |
| --- | --- |
| BPJS Kesehatan | BPJS_KESEHATAN |


## 6.12 PBB

**Request Inquiry PBB**

```shell
{
  "beneficiaryNumber": "357800000003101001",
  "productCode": "PBBDKI"
}
```

**Response Inquiry PBB**

```shell
{
    "id": 1491,
    "type": "PAJAK_BUMI_BANGUNAN",
    "productCode": "PBBDKI",
    "beneficiaryNumber": "357800000003101001",
    "farePrice": 190000.00,
    "minimumPayment": 192500.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 192500.00,
    "margin": 0.00,
    "grandTotal": 192500.00,
    "message": null,
    "createdAt": "2025-03-10T16:19:50.093219",
    "detailAdditionalData": [
        {
            "key": "beneficiaryNumber",
            "value": "357800000003101001",
            "label": "Nomor Objek Pajak"
        },
        {
            "key": "beneficiaryName",
            "value": "WISHNU EKA SIDHARTA",
            "label": "Nama Wajib Pajak"
        },
        {
            "key": "year",
            "value": "2018",
            "label": "Tahun Pajak"
        },
        {
            "key": "productName",
            "value": "PBB DKI Jakarta",
            "label": "Daerah Pajak"
        },
        {
            "key": "subDistrict",
            "value": "Poncokusumo",
            "label": "Kecamatan"
        },
        {
            "key": "surfaceArea",
            "value": "1519 m2",
            "label": "Luas Tanah"
        },
        {
            "key": "buildingArea",
            "value": "119 m2",
            "label": "Luas Bangunan"
        },
        {
            "key": "dueDate",
            "value": "31 Agt 2019",
            "label": "Jatuh Tempo"
        },
        {
            "key": "basicPrice",
            "value": "Rp190.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

**Request Transaksi PBB**

```shell
{
  "inquiryId": "1422",
  "partnerReferenceNo": "1A23F45",
}
```

**Response Transaksi PBB**

```shell
{
    "id": 1765,
    "transactionNumber": "INV/BIL/PAJ/240314/007ABADQ733",
    "type": "PAJAK_BUMI_BANGUNAN",
    "productCode": "PBBDKI",
    "beneficiaryNumber": "357800000003101001",
    "farePrice": 190000.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 192500.00,
    "margin": 0.00,
    "grandTotal": 192500.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1491,
    "partnerReferenceNo": "1A23F45",
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PAJ/240314/007ABADQ733",
            "label": "No. Transaksi"
        },
        {
            "key": "beneficiaryNumber",
            "value": "357800000003101001",
            "label": "Nomor Objek Pajak"
        },
        {
            "key": "beneficiaryName",
            "value": "WISHNU EKA SIDHARTA",
            "label": "Nama Wajib Pajak"
        },
        {
            "key": "year",
            "value": "2018",
            "label": "Tahun Pajak"
        },
        {
            "key": "productName",
            "value": "PBB DKI Jakarta",
            "label": "Daerah Pajak"
        },
        {
            "key": "subDistrict",
            "value": "Poncokusumo",
            "label": "Kecamatan"
        },
        {
            "key": "surfaceArea",
            "value": "1519 m2",
            "label": "Luas Tanah"
        },
        {
            "key": "buildingArea",
            "value": "119 m2",
            "label": "Luas Bangunan"
        },
        {
            "key": "dueDate",
            "value": "31 Agt 2019",
            "label": "Jatuh Tempo"
        },
        {
            "key": "basicPrice",
            "value": "Rp190.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

**Response Mendapatkan Status PBB**

```shell
{
    "id": 1765,
    "transactionNumber": "INV/BIL/PAJ/240314/007ABADQ733",
    "type": "PAJAK_BUMI_BANGUNAN",
    "productCode": "PBBDKI",
    "beneficiaryNumber": "357800000003101001",
    "farePrice": 190000.00,
    "serviceCharge": 2500.00,
    "discount": 0.00,
    "totalPrice": 192500.00,
    "margin": 0.00,
    "grandTotal": 192500.00,
    "message": null,
    "status": "SUCCESS",
    "inquiryId": 1491,
    "partnerReferenceNo": "1A23F45",
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/PAJ/240314/007ABADQ733",
            "label": "No. Transaksi"
        },
        {
            "key": "beneficiaryNumber",
            "value": "357800000003101001",
            "label": "Nomor Objek Pajak"
        },
        {
            "key": "beneficiaryName",
            "value": "WISHNU EKA SIDHARTA",
            "label": "Nama Wajib Pajak"
        },
        {
            "key": "year",
            "value": "2018",
            "label": "Tahun Pajak"
        },
        {
            "key": "productName",
            "value": "PBB DKI Jakarta",
            "label": "Daerah Pajak"
        },
        {
            "key": "subDistrict",
            "value": "Poncokusumo",
            "label": "Kecamatan"
        },
        {
            "key": "surfaceArea",
            "value": "1519 m2",
            "label": "Luas Tanah"
        },
        {
            "key": "buildingArea",
            "value": "119 m2",
            "label": "Luas Bangunan"
        },
        {
            "key": "dueDate",
            "value": "31 Agt 2019",
            "label": "Jatuh Tempo"
        },
        {
            "key": "basicPrice",
            "value": "Rp190.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

Produk PBB adalah produk pascabayar pembayaran pajak bangunan.


| Label | Type Code |
| --- | --- |
| PBB | PAJAK_BUMI_BANGUNAN |


## 6.13 Gas

**Request Inquiry Gas**

```shell
{
  "beneficiaryNumber": "0126511001",
  "productCode": "GAPOPER0000"
}
```

**Response Inquiry Gas**

```shell
{
    "id": 3111,
    "type": "GAS",
    "productCode": "GAPOPER0000",
    "beneficiaryNumber": "0126511001",
    "farePrice": 55000.00,
    "minimumPayment": 56000.00,
    "serviceCharge": 1000.00,
    "discount": 0.00,
    "totalPrice": 56000.00,
    "margin": 0.00,
    "grandTotal": 56000.00,
    "message": null,
    "createdAt": "2025-03-11T09:01:24.592486",
    "detailAdditionalData": [
        {
            "key": "beneficiaryNumber",
            "value": "0126511001",
            "label": "Nomor Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "WISHNU EKA SIDHARTA",
            "label": "Nama"
        },
        {
            "key": "paymentPeriod",
            "value": "Maret 2025",
            "label": "Periode"
        },
        {
            "key": "gasConsumption",
            "value": "12 M3",
            "label": "Volume Pemakaian"
        },
        {
            "key": "basicPrice",
            "value": "Rp55.000",
            "label": "Jumlah Tagihan"
        }
    ],
    "status": "SUCCESS",
    "paymentType": "STATIC"
}
```

**Request Transaksi Gas**

```shell
{
  "inquiryId": "3111",
  "partnerReferenceNo": "1A23F45",
}
```

**Response Transaksi Gas**

```shell
{
    "id": 5370,
    "type": "GAS",
    "productCode": "GAPOPER0000",
    "transactionNumber": "INV/BIL/GAS/250311/0092J09UZU7",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "0126511001",
    "farePrice": 55000.00,
    "serviceCharge": 1000.00,
    "discount": 0.00,
    "totalPrice": 56000.00,
    "margin": 0.00,
    "grandTotal": 56000.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 3111,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/GAS/250311/0092J09UZU7",
            "label": "No. Transaksi"
        },
        {
            "key": "beneficiaryNumber",
            "value": "0126511001",
            "label": "Nomor Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "WISHNU EKA SIDHARTA",
            "label": "Nama"
        },
        {
            "key": "paymentPeriod",
            "value": "Maret 2025",
            "label": "Periode"
        },
        {
            "key": "gasConsumption",
            "value": "12 M3",
            "label": "Volume Pemakaian"
        },
        {
            "key": "basicPrice",
            "value": "Rp55.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

**Response Mendapatkan Status Gas**

```shell
{
    "id": 5370,
    "type": "GAS",
    "productCode": "GAPOPER0000",
    "transactionNumber": "INV/BIL/GAS/250311/0092J09UZU7",
    "partnerReferenceNo": "1A23F45",
    "beneficiaryNumber": "0126511001",
    "farePrice": 55000.00,
    "serviceCharge": 1000.00,
    "discount": 0.00,
    "totalPrice": 56000.00,
    "margin": 0.00,
    "grandTotal": 56000.00,
    "message": null,
    "status": "PENDING",
    "inquiryId": 3111,
    "detailAdditionalData": [
        {
            "key": "transactionNumber",
            "value": "INV/BIL/GAS/250311/0092J09UZU7",
            "label": "No. Transaksi"
        },
        {
            "key": "beneficiaryNumber",
            "value": "0126511001",
            "label": "Nomor Pelanggan"
        },
        {
            "key": "beneficiaryName",
            "value": "WISHNU EKA SIDHARTA",
            "label": "Nama"
        },
        {
            "key": "paymentPeriod",
            "value": "Maret 2025",
            "label": "Periode"
        },
        {
            "key": "gasConsumption",
            "value": "12 M3",
            "label": "Volume Pemakaian"
        },
        {
            "key": "basicPrice",
            "value": "Rp55.000",
            "label": "Jumlah Tagihan"
        }
    ]
}
```

Produk Gas adalah produk pascabayar pembayaran Gas negara.


| Label | Type Code |
| --- | --- |
| Gas | GAS |


## 7. Product List

Fitur ini digunakan untuk menampilkan list produk

### 7.1 Protocol & Service Address

**contoh cURL request body pada product list partner**

```shell
curl --location 'https://sandbox.astrapay.com/v1/mitra-service/products?type=TOKEN_LISTRIK' \
--header 'x-partner-id: 1d7f6c22-5834-4f8b-a2db-165937275d96' \
--header 'x-timestamp: 2024-04-02T10:39:03+07:00' \
--header 'x-signature: 3qplv0ujA+584neae0QOqnbM1wm1kJkjpKQIwzV8eUtQd2Fe//ZSBL4IGynk6Ncw0NoLHG0op56+ob6n0EJigQ==' \
--header 'x-external-id: ASTRAPAYTESTINGSANDBOX12345678900000' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3NTM0MzI1NTksImlhdCI6MTc1MzQzMjI1OSwianRpIjoiZDcyMWJkMDctNjEwMS00ZTJjLTg2MTItY2VkYjhiNTY4NDlkIiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6ImUyMzQwYzFlLTI5ZTgtNDFmMi1iMDFiLTBmMWFiMzcxMjdjOCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjFkN2Y2YzIyLTU4MzQtNGY4Yi1hMmRiLTE2NTkzNzI3NWQ5NiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIzNC4xMDEuMjE2LjE2OCIsImNsaWVudElkIjoiMWQ3ZjZjMjItNTgzNC00ZjhiLWEyZGItMTY1OTM3Mjc1ZDk2IiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LTFkN2Y2YzIyLTU4MzQtNGY4Yi1hMmRiLTE2NTkzNzI3NWQ5NiIsImNsaWVudEFkZHJlc3MiOiIzNC4xMDEuMjE2LjE2OCJ9.DodKl7nWeuxRfQgvk8uKfEmLg7v2Jg3FeHTBVmoRqdP6SGHqxK_CTOgGC7bv3-Q5WoFOYQId9tTgnv5FPS7DzLaJnSeyxNSuZcudcVquvFNiXxyX9d7XGiKWD0ErQSvs7vtqhNnmzxe85MLFq9q5V0HgV0auHfxYwUerX7vhcZUN1_4Ci6220UwipPwZeojMXDUeGGFfXNq2wXvuvIJhnVXNnUjv5HUQhzwcuuFzpW7nCwIT3VRxUdmIsbDr7m0wN3_w37RkTkG5YOy4zOdk_D5Qd9PP5V7rPK538yjs_KnmCrdWcZ4Y-TFZFNcjJsM0BqRqPAWQ0A6fyBGvtHS1lg' \
--header 'Content-Type: application/json' \
```


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | GET |
| URL Sandbox | /v1/mitra-service/products |


### 7.2 Parameters


| Key | Type | Requirement | Description |
| --- | --- | --- | --- |
| size | Int | Optional | Jumlah data yang ditampilkan per halaman (digunakan untuk pagination) |
| page | Int | Optional | Nomor halaman data yang ingin ditampilkan (digunakan bersama dengan size) |
| type | String | Optional | Parameter untuk filter data berdasarkan tipe produk. Informasi kode tipe dapat dilihat pada masing-masing menu produk (contoh: ANGSURAN, TOKEN_LISTRIK, dll.) |
| productCode | String | Optional | Parameter untuk filter data berdasarkan kode produk tertentu |
| name | String | Optional | Parameter untuk filter data berdasarkan nama produk |
| status | String | Optional | Parameter untuk filter data berdasarkan status produk (ACTIVE/INACTIVE). |


### 7.3 Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk mengakses API AstraPay hasil dari generate Signature Service |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| Authorization | String | Mandatory | Bearer token hasil generate dari [API Access Token B2B](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |


### 7.4 Response Body


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| id | Long | Mandatory | Kode unik produk |
| productCode | String | Mandatory | Kode untuk produk (Contoh: PUPRTEL011K) |
| provider | Object | Mandatory | Data provider dari sebuah produk |
| provider.name | String | Mandatory | Nama provider dari sebuah produk |
| provider.code | String | Mandatory | Kode unik provider dari sebuah produk |
| provider.paymentType | String | Mandatory | Tipe pembayaran dari sebuah produk (DYNAMIC / STATIC) |
| name | String | Mandatory | Nama dari sebuah produk |
| status | String | Mandatory | Status dari sebuah produk (ACTIVE / INACTIVE) |
| totalPrice | Int | Mandatory | Nominal harga jual dari AstraPay |
| margin | Int | Mandatory | Keuntungan yang didapatkan Merchant |
| grandTotal | Int | Mandatory | Nominal harga jual yang ditambahkan margin (Rumus: totalPrice + margin) |
| transactionType | Object | Mandatory | Data tipe transaksi dari sebuah produk |
| transactionType.type | String | Mandatory | Tipe transaksi yang ada pada AstraPay(ANGSURAN, PULSA, PAKET_DATA, PASCABAYAR, dan lain-lain) |


### 7.5 Response Status Code

**contoh response product**

```shell
{
    "content": [
        {
            "id": 111,
            "productCode": "PUPRPAKETDATA",
            "provider": {
                "name": "Telkomsel",
                "code": "TELKOMSEL",
                "paymentType": "STATIC"
            },
            "name": "PAKET DATA TELKOMSEL 20 MB",
            "status": "ACTIVE",
            "totalPrice": 25010,
            "margin": 0,
            "grandTotal": 25010,
            "transactionType": {
                "type": "PAKET_DATA",
                "description": "Pulsa"
            }
        },
        {
            "id": 104,
            "productCode": "PUPPTELHALO",
            "provider": {
                "name": "Telkomsel",
                "code": "TELKOMSEL",
                "paymentType": "STATIC"
            },
            "name": "Telkomsel HALO",
            "status": "ACTIVE",
            "totalPrice": 0,
            "margin": 0,
            "grandTotal": 0,
            "transactionType": {
                "type": "PASCABAYAR",
                "description": "Pascabayar"
            }
        },
        {
            "id": 108,
            "productCode": "PUPRTHR001K",
            "provider": {
                "name": "THREE",
                "code": "THREE",
                "paymentType": "STATIC"
            },
            "name": "Three 1K",
            "status": "ACTIVE",
            "totalPrice": 1700,
            "margin": 0,
            "grandTotal": 1700,
            "transactionType": {
                "type": "PULSA",
                "description": "Pulsa"
            }
        },
        {
            "id": 103,
            "productCode": "PUPRTEL025K",
            "provider": {
                "name": "Telkomsel",
                "code": "TELKOMSEL",
                "paymentType": "STATIC"
            },
            "name": "Telkomsel 25rb",
            "status": "ACTIVE",
            "totalPrice": 24550,
            "margin": 250,
            "grandTotal": 24800,
            "transactionType": {
                "type": "PULSA",
                "description": "Pulsa"
            }
        },
        {
            "id": 109,
            "productCode": "PUPRSMA020K",
            "provider": {
                "name": "SMARTFREN",
                "code": "SMARTFREN",
                "paymentType": "STATIC"
            },
            "name": "Smartfren 20K",
            "status": "ACTIVE",
            "totalPrice": 20000,
            "margin": 0,
            "grandTotal": 20000,
            "transactionType": {
                "type": "PULSA",
                "description": "Pulsa"
            }
        },
        {
            "id": 72,
            "productCode": "FIF",
            "provider": {
                "name": "FIFGROUP",
                "code": "FIF",
                "paymentType": "DYNAMIC"
            },
            "name": "Biller FIF",
            "status": "ACTIVE",
            "totalPrice": 0,
            "margin": 0,
            "grandTotal": 0,
            "transactionType": {
                "type": "ANGSURAN",
                "description": "Angsuran"
            }
        },
        {
            "id": 70,
            "productCode": "ACC",
            "provider": {
                "name": "ACC",
                "code": "ACC",
                "paymentType": "STATIC"
            },
            "name": "ACC",
            "status": "ACTIVE",
            "totalPrice": 0,
            "margin": 0,
            "grandTotal": 0,
            "transactionType": {
                "type": "ANGSURAN",
                "description": "Angsuran"
            }
        },
        {
            "id": 105,
            "productCode": "PLPRPLN020K",
            "provider": {
                "name": "PLN",
                "code": "PLN",
                "paymentType": "STATIC"
            },
            "name": "20.000",
            "status": "ACTIVE",
            "totalPrice": 21000,
            "margin": 0,
            "grandTotal": 21000,
            "transactionType": {
                "type": "TOKEN_LISTRIK",
                "description": "Token Listrik"
            }
        },
        {
            "id": 106,
            "productCode": "PLPRPLN050K",
            "provider": {
                "name": "PLN",
                "code": "PLN",
                "paymentType": "STATIC"
            },
            "name": "50.000",
            "status": "ACTIVE",
            "totalPrice": 50400,
            "margin": 0,
            "grandTotal": 50400,
            "transactionType": {
                "type": "TOKEN_LISTRIK",
                "description": "Token Listrik"
            }
        },
        {
            "id": 69,
            "productCode": "MAUCASH",
            "provider": {
                "name": "MAUCASH",
                "code": "MAUCASH",
                "paymentType": "STATIC"
            },
            "name": "MAUCASH",
            "status": "ACTIVE",
            "totalPrice": 0,
            "margin": 0,
            "grandTotal": 0,
            "transactionType": {
                "type": "ANGSURAN",
                "description": "Angsuran"
            }
        },
        {
            "id": 110,
            "productCode": "PUPRTEL020K",
            "provider": {
                "name": "Telkomsel",
                "code": "TELKOMSEL",
                "paymentType": "STATIC"
            },
            "name": "Telkomsel 20rb",
            "status": "ACTIVE",
            "totalPrice": 20200,
            "margin": 800,
            "grandTotal": 21000.0000,
            "transactionType": {
                "type": "PULSA",
                "description": "Pulsa"
            }
        }
```


| Status | Description |
| --- | --- |
| 2xx | Partner harus memberikan response status code 2xx |


## 8. Product Notification

Fitur ini akan memberi tahu partner jika ada perubahan harga dari AstraPay.

### 8.1 Protocol & Service Address

**contoh cURL request body pada product notification partner**

```shell
curl --location 'https://partner.com/product/callback' \
--header 'x-timestamp: 2020-01-01T00:00:00+07:00' \
--header 'Content-Type: application/json' \
--data ' 
{
  "id": 680,
  "productCode": "PDPOPDA0000",
  "provider": {
    "name": "PDAM",
    "code": "PDAM",
    "paymentType": "STATIC"
  },
  "name": "PDAM",
  "status": "ACTIVE",
  "totalPrice": 24150,
  "margin": 0,
  "grandTotal": 24150,
  "transactionType": {
    "type": "TAGIHAN_AIR",
    "description": "Tagihan Air"
  }
}'
```


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | POST |


### 8.2 Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |


### 8.3 Request Body


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| id | Int | Mandatory | Kode unik produk |
| productCode | String | Mandatory | Kode untuk produk (Contoh: PUPRTEL011K) |
| provider | Object | Mandatory | Data provider dari sebuah produk |
| provider.name | String | Mandatory | Nama provider dari sebuah produk |
| provider.code | String | Mandatory | Kode unik provider dari sebuah produk |
| provider.paymentType | String | Mandatory | Tipe pembayaran dari sebuah produk (DYNAMIC / STATIC) |
| name | String | Mandatory | Nama dari sebuah produk |
| status | String | Mandatory | Status dari sebuah produk (ACTIVE / INACTIVE) |
| totalPrice | Int | Mandatory | Nominal harga jual dari AstraPay |
| margin | Int | Mandatory | Keuntungan yang didapatkan Merchant |
| grandTotal | Int | Mandatory | Nominal harga jual yang ditambahkan margin (Rumus: totalPrice + margin) |
| transactionType | Object | Mandatory | Data tipe transaksi dari sebuah produk |
| transactionType.type | String | Mandatory | Tipe transaksi yang ada pada AstraPay(ANGSURAN, PULSA, PAKET_DATA, PASCABAYAR, dan lain-lain) |
| transactionType.description | String | Mandatory | Deskripsi dari sebuah tipe transaksi |


### 8.4 Response Status Code


| Status | Description |
| --- | --- |
| 2xx | Partner harus memberikan response status code 2xx |


## 9. Callback

Dibawah ini merupakan spesifikasi API Callback yang harus disiapkan oleh merchant untuk mendapatkan pembaruan status transaksi.

### 9.1 Protocol & Service Address

**contoh cURL request body pada callback partner**

```shell
curl --location 'https://partner.com/transaction/callback' \
--header 'x-timestamp: 2020-01-01T00:00:00+07:00' \
--header 'Content-Type: application/json' \
--data '
{
                "id": 1762,
                "transactionNumber": "INV/BIL/ASU/240314/008OORD2ATP",
                "type": "BPJS_KESEHATAN",
                "productCode": "ASPOBPJ0000",
                "beneficiaryNumber": "8888802883963273",
                "totalPrice": 72500,
                "margin": 0,
                "grandTotal": 72500,
                "message": null,
                "status": "SUCCESS",
                "inquiryId": 1422,
                "partnerReferenceNo": "1A23F45",
                "detailAdditionalData": [
                    {
                        "key": "transactionNumber",
                        "value": "INV/BIL/ASU/240314/008OORD2ATP",
                        "label": "No. Transaksi"
                    },
                    {
                        "key": "referenceNumberProvider",
                        "value": "8BC3CC22FE669563",
                        "label": "No. Referensi"
                    },
                    {
                        "key": "transactionType",
                        "value": "BPJS Kesehatan",
                        "label": "Jenis Pembayaran"
                    },
                    {
                        "key": "beneficiaryNumber",
                        "value": "8888802883963273",
                        "label": "No. BPJS Kesehatan"
                    },
                    {
                        "key": "beneficiaryName",
                        "value": "Jarister Edwins Silalahi",
                        "label": "Nama Pemilik"
                    },
                    {
                        "key": "totalMembers",
                        "value": "2 orang",
                        "label": "Anggota Keluarga"
                    },
                    {
                        "key": "totalPeriods",
                        "value": "1 bulan",
                        "label": "Periode Bayar"
                    },
                    {
                        "key": "basicPrice",
                        "value": "Rp70",
                        "label": "Jumlah Tagihan"
                    }
                ]
            }
'
```


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | POST |


### 9.2 Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |


### 9.3 Request Body


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| id | Int | Mandatory | Kode unik hasil transaksi |
| transactionNumber | Int | Mandatory | Kode unik transaksi yang di generate oleh AstraPay |
| type | String | Mandatory | Kode untuk kategori produk (Contoh: PULSA) |
| productCode | String | Mandatory | Kode untuk produk (Contoh: PUPRTEL011K) |
| totalPrice | Int | Mandatory | Nominal harga jual dari AstraPay |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |
| beneficiaryNumber | String | Mandatory | Data yang diinput di form field (Contoh: Nomor telepon, Token listrik, dst.) |
| margin | Int | Mandatory | Keuntungan yang didapatkan Merchant |
| grandTotal | Int | Mandatory | Nominal harga jual yang ditambahkan margin (Rumus: *totalPrice + margin*) |
| message | String | Optional | Pesan yang dimunculkan ketika terjadi error |
| status | String | Mandatory | Status Transaksi (SUCCESS / PENDING / VOID ) |
| inquiryId | String | Mandatory | Kode unik hasil inquiry untuk transaksi |
| detailAdditionalData | Object | Mandatory | Data tambahan dari inquiry |
| detailAdditionalData.key | String | Optional | Kata kunci untuk penunjuk data |
| detailAdditionalData.value | String | Optional | Response data |
| detailAdditionalData.label | String | Optional | Label keterangan data |


### 9.4 Response Status Code


| Status | Description |
| --- | --- |
| 2xx | Partner harus memberikan response status code 2xx |


## 10. Biller Response List

Dibawah ini merupakan daftar response code yang akan muncul. Format “xx” dalam Response Code diisikan dengan Service Code dari masing-masing API.

### 10.1 Authentication

**Contoh Response error Autentikasi**

```shell
{
    "responseCode": "4017300",
    "responseMessage": "Unauthorized. Invalid Client ID or Client Secret or Signature"
}
```


| Response Code | Response Message | Description |
| --- | --- | --- |
| 404xx08 | Invalid Merchant | Client ID atau Merchant ID tidak ditemukan |
| 200xx00 | Successful | Sukses |
| 400xx00 | Bad Request | Pesan umum yang akan diterima ketika permintaan gagal/tidak sesuai |
| 500xx00 | General Error | General Error |
| 401xx01 | Invalid Token (B2B) | Request token invalid |
| 401xx00 | Unauthorized. Signature | Invalid signature |
| 409xx00 | Conflict | Menggunakan X-EXTERNAL-ID yang sama di hari yang sama |
| 400xx01 | Invalid Field Format X-TIMESTAMP | Format field X-TIMESTAMP salah |
| 400xx01 | Invalid Field Format X-EXTERNAL-ID | Format field X-EXTERNAL-ID salah |
| 400xx02 | Invalid Field Format X-TIMESTAMP | X-TIMESTAMP tidak ada atau tidak lengkap |
| 400xx02 | Invalid Field Format X-EXTERNAL-ID | X-EXTERNAL-ID tidak ada atau tidak lengkap |
| 400xx02 | Invalid Field Format X-PARTNER-ID | X-PARTNER-ID tidak ada atau tidak lengkap |


### 10.2 Inquiry

**Contoh Response Error Inquiry dengan code BILL_ALREADY_PAID**

```shell
{
            "status": 400,
            "message": "Tagihan sudah terbayar.",
            "error": "Bad Request",
            "path": "/v1/mitra-service/inquiries",
            "details": [
                {
                    "code": "BILL_ALREADY_PAID",
                    "objectName": null,
                    "defaultMessage": "Tagihan sudah terbayar.",
                    "field": null,
                    "rejectedValue": null,
                    "additionalData": []
                }
            ]
        }
```


| Status | Code | Description |
| --- | --- | --- |
| 200 |  | Inquiry terdaftar |
| 201 |  | Inquiry berhasil terbuat |
| 400 | UserIsAgent | User bukan merupakan Merchant |
| 400 | ProductIsActive | Produk sedang tidak aktif |
| 400 | InsufficientBalance | Saldo Merchant tidak mencukupi |
| 400 | CONTRACT_NOT_FOUND | Beneficiary number tidak ditemukan yang dapat disebabkan oleh nomor sudah tidak terdaftar, tidak aktif, atau terdapat kesalahan dalam pengetikan (typo) |
| 400 | BILL_ALREADY_PAID | Tagihan sudah dibayarkan dan tagihan yang baru belum terbit atau belum bisa dibayarkan |
| 400 | PRODUCT_NOT_EXIST | Produk sedang tidak tersedia |
| 400 | OPERATOR_ISSUE | Kegagalan yang terjadi yang disebabkan oleh operator |
| 400 | CUT_OFF | Kegagalan yang terjadi karena di luar jam operasional, biasanya berasal dari operator yang seringnya terjadi ketika transaksi tengah malam |
| 400 | LAST_INSTALLMENT_FIF | Pembayaran tidak dapat dilakukan melalui sistem AstraPay karena merupakan angsuran terakhir sehingga harus datang langsung ke cabang FIFGROUP |
| 400 | LAST_INSTALLMENT_TAF | Pembayaran tidak dapat dilakukan melalui sistem AstraPay karena merupakan angsuran terakhir sehingga harus datang langsung ke cabang TAF |
| 400 | OVERDUE_TAF | Pembayaran tidak dapat dilakukan melalui sistem AstraPay, karena merupakan angsuran yang jatuh tempo sehingga harus datang langsung ke cabang TAF |
| 400 | BLOCKED_NUMBER | Pembayaran tidak dapat dilakukan karena nomor telah diblokir oleh operator |
| 400 | SYSTEM_FAILED | Error sistem terkait server, IP, dan lain-lain yang seharusnya dapat diperbaiki |
| 400 | FAILED | Error general yang tidak dapat diketahui jelas penyebab errornya |


### 10.3 Transaction

**Contoh Response Error Transaction dengan code InquiryExists**

```shell
{
            "status": 400,
            "message": "save.requestDto.inquiryId: Inquiry not found.",
            "error": "Bad Request",
            "path": "/v1/mitra-service/transactions",
            "timestamp": "2023-12-14T15:06:24.985970",
            "details": [
                {
                    "code": "InquiryExists",
                    "objectName": "InquiryExists",
                    "defaultMessage": "Inquiry not found.",
                    "field": "inquiryId",
                    "rejectedValue": 1235,
                    "additionalData": null
                }
            ]
            }
```


| Status | Code | Description |
| --- | --- | --- |
| 200 |  | Transaksi terdaftar |
| 201 |  | Transaksi berhasil terbuat |
| 400 | InquiryExists | Hasil inquiry tidak ditemukan |
| 400 | UserIsAgent | User bukan merupakan Merchant |
| 400 | ProductIsActive | Produk sedang tidak aktif |
| 400 | OPERATOR_ISSUE | Kegagalan yang terjadi yang disebabkan oleh operator |
| 400 | CUT_OFF | Kegagalan yang terjadi karena di luar jam operasional, biasanya berasal dari operator yang seringnya terjadi ketika transaksi tengah malam |
| 400 | KWH_EXCEED_LIMIT | Jumlah KWH (kilowatt-hour) melebihi batasan yang telah ditentukan PLN (biasanya di token listrik) dan setiap pelanggan batasannya berbeda-beda |
| 400 | SYSTEM_FAILED | Error sistem terkait server, IP, dan lain-lain yang seharusnya dapat diperbaiki |
| 400 | FAILED | Error general yang tidak dapat diketahui jelas penyebab errornya |


### 10.4 Get Transaction

**Contoh Response Error Mendapatkan Transaksi dengan Code IsMyTransaction**

```shell
{
            "status": 400,
            "message": "getTransaction.id: It is not your transaction",
            "error": "Bad Request",
            "path": "/v1/mitra-service/transactions/17",
            "timestamp": "2025-03-13T08:46:51.817956",
            "details": [
                {
                    "code": "IsMyTransaction",
                    "objectName": "IsMyTransaction",
                    "defaultMessage": "It is not your transaction",
                    "field": "id",
                    "rejectedValue": 17,
                    "additionalData": null
                }
            ]
        }
```


| Status | Code | Description |
| --- | --- | --- |
| 200 |  | Transaksi ada dan berhasil didapatkan |
| 400 | IsMyTransaction | Transaksi milik Merchant yang unik dan tidak dapat diakses oleh Merchant lain |
| 400 | SYSTEM_FAILED | Error sistem terkait server, IP, dan lain-lain yang seharusnya dapat diperbaiki |
| 400 | FAILED | Error general yang tidak dapat diketahui jelas penyebab errornya |


### 10.5 Status Transaction Checking

**Contoh Response Error Saat Melakukan Pengecekan Status Transaksi dengan Code PartnerReferenceNoExist**

```shell
{
                   "status": 400,
                    "message": "getTransactionStatus.transactionRequestDto: Partner reference number is not found",
                    "error": "Bad Request",
                    "path": "/v1/mitra-service/transactions/status",
                    "timestamp": "2025-09-12T10:14:38.683950",
                    "details": [
                        {
                            "code": "PartnerReferenceNoExist",
                            "objectName": "PartnerReferenceNoExist",
                            "defaultMessage": "Partner reference number is not found",
                            "field": "",
                            "rejectedValue": {
                                "partnerReferenceNo": "randomref12345",
                                "transactionDateTime": "2023-02-06T14:58:54"
                            },
                            "additionalData": null
                        }
                    ]
                }
```


| Status | Code | Description |
| --- | --- | --- |
| 200 |  | Transaksi ada dan berhasil didapatkan |
| 400 | PartnerReferenceNoExist | Transaksi dengan reference number yang dimasukkan tidak ditemukan |
| 400 | SYSTEM_FAILED | Error sistem terkait server, IP, dan lain-lain yang seharusnya dapat diperbaiki |
| 400 | FAILED | Error general yang tidak dapat diketahui jelas penyebab errornya |

