# QRIS Dynamic SNAP BI V1.0


## Overview SNAP BI

Selamat datang di dokumentasi AstraPay QRIS Dynamic dengan standar SNAP BI.

Dokumentasi ini menjelaskan *procedure acceptance* untuk implementasi QRIS Dynamic dari perspektif merchant.

Saat ini, AstraPay QRIS Dynamic menyediakan metode integrasi melalui SNAP AstraPay API. Dimana SNAP (Standar Nasional Open API Pembayaran) adalah standar Open API yang ditetapkan Bank Indonesia agar menciptakan industri sistem pembayaran yang lebih maju di Indonesia.

### Glosarium

Sebelum melakukan integrasi, mari kita bahas terlebih dahulu beberapa definisi dari istilah yang akan muncul pada dokumentasi ini. Penjelasan dari istilah tersebut adalah sebagai berikut:


| Istilah | Deskripsi |
| --- | --- |
| QRIS | QRIS (Quick Response Code Indonesian Standard) adalah standar kode QR yang dikembangkan di Indonesia untuk memfasilitasi transaksi pembayaran elektronik. QRIS memungkinkan pengguna untuk melakukan pembayaran menggunakan kode QR yang dapat dibaca oleh aplikasi pembayaran. |
| QRIS Dynamic | Varian dari QRIS yang memungkinkan pembayaran tagihan yang spesifik menggunakan kode QR yang unik. Dalam QRIS Dynamic, QR code yang dihasilkan berisi informasi tagihan yang spesifik, seperti jumlah pembayaran, nomor referensi, dan detail transaksi lainnya. |
| Switcher | Pihak yang berperan sebagai perantara atau penghubung antara Penyedia Jasa Pembayaran (PJP) dengan institusi keuangan penerima atau pedagang (merchant) dan bertugas untuk memfasilitasi transaksi pembayaran yang menggunakan QRIS antara berbagai PJP. |
| Issuer | Lembaga keuangan yang mengeluarkan atau menerbitkan alat pembayaran elektronik kepada pengguna yang nantinya dapat digunakan untuk melakukan transaksi pembayaran menggunakan QRIS. |
| Acquirer | Entitas atau sistem yang bekerja sama dengan pedagang (merchant) untuk menerima pembayaran melalui QRIS. Acquirer bertugas untuk menyediakan teknologi dan infrastruktur yang dibutuhkan untuk menerima pembayaran melalui QRIS, seperti menyediakan perangkat pembaca QR, mengelola sistem pembayaran, dan menghubungkan pedagang dengan penyedia layanan pembayaran elektronik. |
| Rekonsiliasi | Proses membandingkan data transaksi yang dicatat oleh pedagang (merchant) dengan data transaksi yang diterima oleh PJP. Hal ini bertujuan untuk memastikan bahwa semua transaksi yang terjadi telah tercatat dengan benar, tidak ada kesalahan, dan tidak ada perbedaan data antara pihak-pihak yang terlibat. |
| Settlement | Proses transfer dana dari rekening yang melakukan pembayaran ke rekening pedagang (merchant) yang menerima pembayaran. Proses settlement ini biasanya melibatkan peran Switcher dan Acquirer, di mana mereka memfasilitasi transfer dana secara elektronik antara rekening-rekening yang terlibat. |
| Bill | Tagihan atau invoice yang dapat dibayarkan menggunakan QRIS. Dalam QRIS Dynamic, QR code yang dihasilkan berisi informasi tagihan yang spesifik, seperti jumlah yang harus dibayarkan, nomor referensi, dan detail transaksi lainnya. |
| MPM | Merchant Presented Mode, merchant menyajikan QR code QRIS kepada pelanggan untuk dipindai menggunakan aplikasi pembayaran yang kompatibel dengan QRIS. |


Berikut ini adalah flow transaksi QRIS Dynamic secara umum.

![img](./assets/umlMpmQrisDynamic-b7043423.png)

Pada dokumentasi ini juga menjelaskan tentang API *QRIS Dynamic* yang diimplementasi pada Gateway.

Berikut adalah beberapa API yang kami sediakan untuk Merchant, yaitu:

1. SNAP Keamanan (Authorization)
2. Generate QR MPM (Create Bill)
3. QR Payment Query (Check Status Bill)
4. Payment Notification (Callback)
5. Refund QR MPM

Dari proses di atas, akan dijelaskan apa saja yang perlu disiapkan dan bagaimana langkah-langkah implementasinya.

## Environment SNAP BI V1.0


| Item | Value |
| --- | --- |
| Development | https://sandbox.astrapay.com |
| Production | URL production akan dikirimkan melalui email setelah UAT selesai dilakukan |


## SNAP Keamanan

Klik [disini](#snap-keamanan) untuk detail informasi SNAP Keamanan AstraPay.

## Generate QR MPM (Create Bill)

API ini berfungsi untuk membuat bill QRIS Dynamic, QRIS Dynamic memiliki waktu expired sesuai dengan **validityPeriod** yang diberikan dengan maksimal 24 jam dari saat QRIS dibuat.

![img](./assets/snapBiCreateQrisDynamic-fdba621c.png)

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 47 |
| Channel ID | 00447 |
| Verb | POST |
| URL | [host]:[port]/qris-service/snap/v1.0/qr/qr-mpm-generate |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari API Token B2B Access Token Request |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format yyyy-MM-ddTHH:mm:ssTZD. Value yang digunakan harus sama dengan value pada signature-auth, token B2B Access Token Request, dan signature-service |
| X-SIGNATURE | String | Mandatory | Signature untuk QR Query Payment hasil dari generate Signature Service. |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari.  Format yang digunakan adalah:  36 Random Numeric String |
| CHANNEL-ID | String | Mandatory | ID dari service API Generate QR MPM |


### Request Body

**Contoh cURL generate QR MPM**

```shell
curl --location --request POST '[host]:[port]/qris-service/snap/v1.0/qr/qr-mpm-generate' \
--header 'Authorization: Bearer xxxbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibUxxx' \
--header 'X-TIMESTAMP: 2022-10-17T20:00:00+07:00' \
--header 'X-SIGNATURE: xxxgt3kVLWyN41MEohgcJ1fTGXiiqWPflMncMr5gnLwblTqMeky1Yfxxx' \
--header 'CHANNEL-ID: 00447' \
--header 'X-PARTNER-ID: astrapay-testing' \
--header 'X-EXTERNAL-ID: 41807553358950093184162180797837' \
--header 'Content-Type: application/json' \
--data-raw '{
   "partnerReferenceNo":"2020102900000000006802",
   "amount":{
      "value":"1.00",
      "currency":"IDR"
   },
   "merchantId":"210000240",
   "terminalId":"A01",
   "validityPeriod":"2022-10-17T20:00:00",
   "additionalInfo":{
       "tip": "0.00"
   }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| partnerReferenceNo | String | Mandatory | ID transaksi dari Merchant/Partner (Merchant/Partner Transaction Id). Unik setiap request. |
| amount | Object | Mandatory | Jumlah Transaksi Net  yang diberikan. Amount memiliki dua sub-field yang tertera di dua kolom di bawah. |
| value | String | Mandatory | Jumlah Net dari transaksi. Contoh: total transaksi IDR10.000, menjadi 10000.00 (2 nol di belakang koma) |
| currency | String | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| merchantId | String | Mandatory | Kode unik setiap merchant |
| terminalId | String | Mandatory | Kode terminal |
| validityPeriod | String | Optional | Periode waktu kedaluwarsa dari satu tagihan.  Ketika validityPeriod tidak diberikan maka secara default akan di set 4 jam setelah QR digenerate.  Validity period tidak boleh lebih dari 24 jam (1 hari) dari waktu generate QR MPM. |
| additionalInfo | Object | Optional | Informasi Tambahan. Terdapat *subfield* yang bisa dilihat di kolom di bawah. |
| tip | String | Mandatory | (ISO4217) Jumlah Net dari tip. Contoh: jika total tip IDR10.000, maka harus ditulis 10000.00 (2 nol di belakang koma). Jika tip tidak ada maka cantumkan “0.00”. |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2004700",
    "responseMessage": "Successful",
    "referenceNo": "BJI33Z8Z0H",
    "partnerReferenceNo": "2020102900000000006718",
    "qrContent": "00020101021226640018ID.CO.ASTRAPAY.WWW011893600822321989939702092198993200303UMI51400014ID.CO.QRIS.WWW0211ID2198993970303UMI5204549953033605404100055020256031005802ID5912Toko Asepso56007JAKARTA61051015062210110BJI33Z8Z0H0703A016304E8D4",
    "merchantName": "Toko Asepso5",
    "terminalId": "A001",
    "additionalInfo": {
        "tip": "100.00"
    }
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| referenceNo | String | Mandatory | Id transaksi dari AstraPay Ketika generate qr mpm berhasil (AstraPay Bill Number) |
| partnerReferenceNo | String | Mandatory | Id transaksi dari Merchant/Partner Ketika generate qr mpm berhasil (Merchant/Partner Bill Number) |
| qrContent | String | Mandatory | QR String MPM |
| merchantName | String | Mandatory | Nama dari Merchant/Partner |
| terminalId | String | Mandatory | Code terminal |
| additionalInfo | Object | Mandatory | Informasi Tambahan. Terdapat subfield yang bisa dilihat di kolom di bawah. |
| tip | String | Mandatory | (ISO4217)  Jumlah Net dari tip. Contoh: jika total tip IDR10.000, maka harus ditulis 10000.00 (2 nol di belakang koma). Jika tip tidak ada maka cantumkan “0.00”. |


> [!NOTE]
> Saat ini API pembayaran QRIS untuk proses testing belum tersedia sehingga perlu menghubungi tim development AstraPay untuk menyelesaikan proses tersebut

## QR Payment Query (Check Status Bill)

API ini berfungsi untuk mengetahui apakah *bill* sudah terbayar atau belum dan apakah *bill* sudah kedaluwarsa (*expired*) atau belum.

- Ketika pembayaran telah berhasil oleh *customer*, merchant dapat mengkonfirmasi Status Transaksi menggunakan API *QR Payment Query* atau menunggu *Callback*
- Pengecekan Status Transaksi pada Sistem Merchant perlu dibuat oleh setiap Merchant untuk membantu kasir mengetahui status transaksi jika Sistem Merchant belum menerima *Callback* dari AstraPay

![img](./assets/QrQueryPayment-2e4be459.png)

### QR Payment Status

Berikut adalah list Bill Status dalam QR Dynamic AstraPay:


| Bill Status | Description |
| --- | --- |
| PENDING | Bill menunggu pembayaran dan masih bisa dibayar selama jangka waktu *validityPeriod* yang diberikan |
| CANCELED | Bill kedaluwarsa setelah melewati *validityPeriod* yang diberikan dan sudah tidak bisa dibayar |
| SUCCESS | Bill sudah terbayar dan tidak bisa dibayar kembali |
| INITIATED | Bill sedang dalam proses pembuatan |
| PAYING | Proses transaksi pembayaran Bill sedang berlangsung |
| FAILED | Bill gagal dibayar |
| REFUND | Bill berhasil di *refund* (untuk pengerjaan berikutnya) |


### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 51 |
| Channel ID | 00551 |
| Verb | GET |
| URL | [host]:[port]/qris-service/snap/v1.0/qr/qr-mpm-query |


**Contoh cURL QR Payment Query**

```shell
curl --location --request POST '[host]:[port]/qris-service/snap/v1.0/qr/qr-mpm-query' \
--header 'Authorization: Bearer xxxiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU9xxx' \
--header 'X-TIMESTAMP: 2022-10-17T20:00:00+07:00' \
--header 'X-SIGNATURE: xxxcJ1fTGXiiqWPflMncMr5gnLwblTqMeky1YfspeFo5fJfagvrHKq1FKHxxx' \
--header 'CHANNEL-ID: 00551' \
--header 'X-PARTNER-ID: astrapay-testing' \
--header 'X-EXTERNAL-ID: 41807553358950093184162180797837' \
--header 'Content-Type: application/json' \
--data-raw '{
   "originalReferenceNo":"ZSE44L79LN",
   "originalPartnerReferenceNo":"2020102900000000000001",
   "originalExternalId":"30443786930722726463280097920912",
   "serviceCode" : "47",
   "merchantId":"A01",
   "additionalInfo":{
      "tip": "0.00"
   }
}'
```

### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari API Token B2B Access Token Request |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format yyyy-MM-ddTHH:mm:ssTZD. Value yang digunakan harus sama dengan value pada signature-auth, token B2B Access Token Request, dan signature-service |
| X-SIGNATURE | String | Mandatory | Signature untuk QR Query Payment hasil dari generate Signature Service. |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari.  Format yang digunakan adalah:  36 Random Numeric String |
| CHANNEL-ID | String | Mandatory | ID dari service QR Query Payment (00551) |


### Request Body


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalReferenceNo | String | Mandatory | Id transaksi dari AstraPay Ketika generate qr mpm berhasil (AstraPay Bill Number) |
| originalPartnerReferenceNo | String | Mandatory | Id transaksi dari Merchant/Partner Ketika generate qr mpm berhasil (Merchant/Partner Bill Number) |
| originalExternalId | String | Optional | External- ID pada header message |
| serviceCode | String | Mandatory | Indikasi tipe transaksi (service code dari original transaction request) |
| merchantId | String | Mandatory | ID Merchant |
| additionalInfo | Object | Optional | Informasi Tambahan. Terdapat subfield yang bisa dilihat di kolom di bawah. |
| tip | String | Mandatory | (ISO4217)  Jumlah Net dari tip. Contoh: jika total tip IDR10.000, maka harus ditulis 10000.00 (2 nol di belakang koma). Jika tip tidak ada maka cantumkan “0.00”. |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2005100",
    "responseMessage": "Successful",
    "originalReferenceNo": "ZSE44L79LN",
    "originalPartnerReferenceNo": "2020102900000000006695",
    "originalExternalId": null,
    "serviceCode": "47",
    "latestTransactionStatus": "00",
    "transactionStatusDesc": "SUCCESS",
    "paidTime": "2022-10-12T11:09:10.912333",
    "amount": {
        "value": "1.00",
        "currency": "IDR"
    },
    "terminalId": "5142",
    "additionalInfo": {
        "tip": "0.00"
        "customerPan": "9360082230001999476"
        "customerReferenceNumber": "497217068001"
        "issuerName": "ASTRAPAY"
    }
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| originalReferenceNo | String | Mandatory | Id transaksi dari AstraPay Ketika generate qr mpm berhasil (AstraPay Bill Number) |
| originalPartnerReferenceNo | String | Mandatory | Id transaksi dari Merchant/Partner Ketika generate qr mpm berhasil (Merchant/Partner Bill Number) |
| originalExternalId | String | Optional | External- ID pada header message |
| serviceCode | String | Mandatory | Indikasi tipe transaksi (service code dari original transaction request) |
| latestTransactionStatus | String | Mandatory | 00 - Success 01 - Initiated 02 - Paying 03 - Pending 04 - Refunded 05 - Canceled 06 - Failed 07 - Not found |
| transactionStatusDesc | String | Optional | Description status transaction |
| paidTime | String | Conditional | Tanggal transaksi |
| amount | Object | Optional | Jumlah Transaksi Net  yang diberikan. Amount memiliki dua sub-field yang tertera di dua kolom di bawah. |
| value | String | Mandatory | Jumlah Net dari transaksi. Contoh: total transaksi IDR10.000, menjadi 10000.00 (2 nol di belakang koma) |
| currency | String | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| terminalId | String | Optional | Terminal ID dari merchant |
| additionalInfo | Object | Optional | Informasi Tambahan. Terdapat subfield yang bisa dilihat di kolom di bawah. |
| tip | String | Mandatory | (ISO4217)  Jumlah Net dari tip. Contoh: jika total tip IDR10.000, maka harus ditulis 10000.00 (2 nol di belakang koma). Jika tip tidak ada maka cantumkan “0.00”. |
| customerPan | String | Mandatory | Identifikasi nomor pelanggan yang melakukan pembayaran melalui QRIS |
| customerReferenceNumber | String | Mandatory | Nomor transaksi yang di-generate oleh pihak Issuer |
| issuerName | String | Mandatory | Informasi nama issuer. |


## Payment Notification (Callback)

Parameter callback pada *request* akan digunakan oleh AstraPay untuk konfirmasi pembayaran yang telah dilakukan oleh customer Anda. Pada saat customer berhasil melakukan pembayaran, AstraPay akan mengirimkan HTTP POST yang menyertakan hasil pembayaran suatu tagihan dari customer. Anda perlu menyediakan halaman untuk menerima *request callback* tersebut. Agar dapat memproses hasil transaksi yang telah dilakukan oleh customer.

![img](./assets/callbackSnapBi-d5942bc7.png)

### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari API Token B2B Access Token Request |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format yyyy-MM-ddTHH:mm:ssTZD. Value yang digunakan harus sama dengan value pada signature-auth, token B2B Access Token Request, dan signature-service |
| X-SIGNATURE | String | Mandatory | Signature untuk QR Query Payment hasil dari generate Signature Service. |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari Partner/Merchant |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari.  Format yang digunakan adalah:  36 Random Numeric String |
| CHANNEL-ID | String | Mandatory | ID dari service QR Query Payment (00752) |


### Protocol dan Service Address


| Item | Value |
| --- | --- |
| Service Code | 52 |
| Channel ID | 00752 |
| Protocol | HTTPS |
| Verb | POST |
| URL | [host]:[port]/snap/v1.0/qr/qr-mpm-notify |
| Relative Path URL | /v1.0/qr/qr-mpm-notify |


### Request Body

**Contoh cURL Callback**

```shell
POST …/snap/v1.0/qr/qr-mpm-notify HTTP/1.2
Content-type: application/json
Authorization: Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a"
X-TIMESTAMP: 2020-12-23T08:46:11+07:00
X-SIGNATURE:
85be817c55b2c135157c7e89f52499bf0c25ad6eeebe04a986e8c8625
61b19a5
X-PARTNER-ID: 82150823919040624621823174737537
X-EXTERNAL-ID: 41807553358950093184162180797837
CHANNEL-ID: 00752
{
  "originalReferenceNo": "XCBS6Z450S",
  "originalPartnerReferenceNo": "DEV20250826422460",
  "latestTransactionStatus": "00",
  "transactionStatusDesc": "SUCCESS",
  "customerNumber": "9360082230250015792",
  "destinationNumber": "9360082232504524231",
  "destinationAccountName": "Asep & Testing",
  "externalStoreId": null,
  "amount": {
    "value": "100.00",
    "currency": "IDR"
  },
  "additionalInfo": {
    "tip": "0.00",
    "deviceId": "AstraPay",
    "channel": "POS",
    "gracePeriod": null,
    "terminalId": "5164",
    "userName": "AstraPay User",
    "merchantPan": "9360082232504524231",
    "customerPan": "9360082230250015792",
    "issuerName": "93600822",
    "transactionId": "INV/QRM/4542BR/250826/2G4Y6CCZ"
  }
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi dari Merchant/Partner (Merchant/Partner Transaction Id) |
| originalReferenceNo | String | Mandatory | AstraPay Transaction ID |
| latestTransactionStatus | String | Mandatory | 00 - Success 01 - Initiated 02 - Paying 03 - Pending 04 - Refunded 05 - Canceled 06 - Failed 07 - Not found |
| transactionStatusDesc | String | Optional | Deskripsi status transaksi |
| customerNumber | String | Optional | Nomor customer yang melakukan pembayaran |
| destinationNumber | String | Optional | Nomor akun tujuan (merchant) |
| destinationAccountName | String | Optional | Nama akun tujuan (merchant) |
| externalStoreId | String | Optional | ID toko eksternal |
| amount | Object | Mandatory | Jumlah Transaksi Net yang diberikan. Amount memiliki dua sub-field yang tertera di dua kolom di bawah |
| value | String | Mandatory | Jumlah Net dari transaksi. Contoh: total transaksi IDR10.000, menjadi 10000.00 (2 nol di belakang koma) |
| currency | String | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| additionalInfo | Object | Mandatory | Informasi Tambahan. Terdapat subfield yang tertera di kolom di bawah |
| tip | String | Mandatory | (ISO4217)   Jumlah Net dari tip. Contoh: jika total tip IDR10.000, maka harus di tulis 10000.00 (2 nol di belakang koma). Jika tip tidak ada maka cantumkan “0.00”. |
| deviceId | String | Optional | ID perangkat yang digunakan untuk transaksi |
| channel | String | Optional | Channel yang digunakan untuk transaksi (contoh: POS) |
| gracePeriod | String | Optional | Masa tenggang untuk transaksi |
| terminalId | String | Optional | ID terminal yang digunakan |
| userName | String | Optional | Nama user yang melakukan transaksi |
| merchantPan | String | Optional | PAN merchant yang menerima pembayaran |
| customerPan | String | Optional | PAN customer yang melakukan pembayaran |
| issuerName | String | Optional | Informasi issuer |
| transactionId | String | Optional | ID transaksi internal AstraPay |


### Response Body

**Contoh Response**

```shell
{
 "responseCode":"2005200",
 "responseMessage":"Request has been processed successfully",
 "additionalInfo":{
    "tip":"10.00",
 }
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| additionalInfo | Object | Optional | Informasi Tambahan. Terdapat subfield yang bisa dilihat di kolom di bawah. |
| tip | String | Mandatory | (ISO4217)    Jumlah Net dari tip. Contoh: jika total tip IDR10.000, maka harus ditulis 10000.00 (2 nol di belakang koma). Jika tip tidak ada maka cantumkan “0.00”. |


> [!NOTE]
> **Catatan :**
> 
> Merchant diharapkan membuat API callback (Payment Notification) sesuai dengan spesifikasi AstraPay
> Merchant diharapkan membuat API B2B Acces Token untuk kebutuhan otorisasi sesuai dengan spesifikasi AstraPay.

## Refund QR MPM

API ini berfungsi untuk Refund dana dari merchant ke Astrapay sebagai Acquirer untuk transaksi yang status SUCCESS.

![img](./assets/snapBiRefundQrMPM-615aa149.png)

### Protocol dan Service Address


| Item | Value |
| --- | --- |
| Service Code | 78 |
| Channel ID | 01878 |
| Protocol | HTTPS |
| Verb | POST |
| URL | [host]:[port]/snap/v1.0/qr/qr-mpm-refund |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari API Token B2B Access Token Request |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format yyyy-MM-ddTHH:mm:ssTZD. Value yang digunakan harus sama dengan value pada signature-auth, token B2B Access Token Request, dan signature-service |
| X-SIGNATURE | String | Mandatory | Signature untuk QR Refund MPM dari generate Signature Service. |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari Partner/Merchant |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari.  Format yang digunakan adalah:  36 Random Numeric String |
| CHANNEL-ID | String | Mandatory | ID dari service QR Refund MPM (01878) |


### Request Body

**Contoh cURL Callback**

```shell
POST …/snap/v1.0/qr/qr-mpm-refund HTTP/1.2
Content-type: application/json
Authorization: Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a"
X-TIMESTAMP: 2024-11-05T08:46:11+07:00
X-SIGNATURE:
85be817c55b2c135157c7e89f52499bf0c25ad6eeebe04a986e8c8625
61b19a5
X-PARTNER-ID: 82150823919040624621823174737537
X-EXTERNAL-ID: 41807553358950093184162180797837
CHANNEL-ID: 01878
{
   "merchantId":"210003702",
   "subMerchantId":"310928924949487",
   "externalStoreId":"124928924949487",
   "originalPartnerReferenceNo":"te1kK8t43581u791",
   "originalReferenceNo":"0MP2OC45X4",
   "originalExternalId":"10052019",
   "partnerRefundNo":"239850918204981205971Z",
   "refundAmount":{
      "value":"2000.00",
      "currency":"IDR"
   },
   "reason":"Customer complain",
   "additionalInfo":null
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| merchantId | String | Mandatory | Kode unik setiap merchant |
| subMerchantId | String | Optional | Sub merchant ID |
| externalStoreId | String | Optional | External Store ID |
| originalPartnerReferenceNo | String | Mandatory | Kode transaksi dari Merchant/Partner |
| originalReferenceNo | String | Mandatory | Kode transaksi dari AstraPay (Bill) |
| originalExternalId | String | Optional | Original Customer / Reference Number |
| partnerRefundNo | String | Mandatory | ID transaksi refund dari Merchant/Partner (Merchant/Partner Transaction Id). Unik setiap request. |
| refundAmount | Object | Mandatory | Informasi terkait nominal yang di minta Refund. |
| reason | String | Mandatory | Alasan melakukan Refund |
| additionalInfo | Object | Optional | Additional information |


### Response Body

**Contoh Response**

```shell
{
   "responseCode":"2007800",
   "responseMessage":"Request has been processed successfully",
   "originalPartnerReferenceNo":"2020102900000000000001",
   "originalReferenceNo":"2020102977770000000009",
   "originalExternalId":"10052019",
   "referenceNo":"REF993883",
   "partnerRefundNo":"239850918204981205970",
   "refundAmount":{
      "value":"10000.00",
      "currency":"IDR"
   },
   "refundTime":"2020-12-21T17:21:41+07:00",
   "additionalInfo":null
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-qris-snap-bi) |
| originalPartnerReferenceNo | String | Mandatory | Kode transaksi dari Merchant/Partner |
| originalReferenceNo | String | Mandatory | Kode transaksi dari AstraPay (Bill) |
| originalExternalId | String | Optional | External Store ID |
| referenceNo | String | Mandatory | Kode transaksi dari AstraPay |
| partnerRefundNo | String | Mandatory | ID transaksi refund dari Merchant/Partner (Merchant/Partner Transaction Id). Unik setiap request. |
| refundAmount | Object | Mandatory | Informasi terkait nominal yang di minta Refund. |
| refundTime | Datetime | Mandatory | Waktu selesai proses Refund |
| additionalInfo | Object | Optional |  |


## Response List


| Response Code | Service Code | Case Code | Response Message | Description |
| --- | --- | --- | --- | --- |
| 200  **OK** | any | 00 | Successful | Sukses melakukan request. |
| 400  **Bad Request** | any | 01 | Invalid Field Format | Format field tidak benar. |
| 400 **Bad Request** | any | 02 | Invalid mandatory field {fieldName} | Data yang dibutuhkan tidak ada atau tidak lengkap. |
| 409 **Duplicate** | 47 | 01 | Duplicate partnerReferenceNo | partnerReferenceNo sudah ada pada sistem. |
| 404 **Not Found** | 47 | 17 | Invalid Terminal | Terminal code tidak ada di sistem. |
| 404 **Not Found** | 51 | 08 | Invalid Merchant | Merchant id tidak ada di sistem atau status merchant *abnormal*. |
| 500 **Internal Server Error** |  |  | Respons tidak valid dari Back-end atau Kesalahan Tidak Terdefinisi |  |

