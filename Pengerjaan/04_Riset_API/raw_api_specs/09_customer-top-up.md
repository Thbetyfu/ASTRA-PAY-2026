# Customer Top Up


## SNAP Introduction

Selamat datang di dokumentasi AstraPay Customer Top Up yang sesuai SNAP BI.

Dokumentasi ini menjelaskan *procedure acceptance* untuk implementasi API Customer Top Up dari perspektif Merchant.

Saat ini, AstraPay Customer Top Up menyediakan metode integrasi melalui SNAP AstraPay API. Dimana SNAP (Standar Nasional Open API Pembayaran) adalah standar Open API yang ditetapkan Bank Indonesia agar menciptakan industri sistem pembayaran yang lebih maju di Indonesia. Dokumen integrasi API Customer Top Up yang digunakan AstraPay merujuk pada bagian Transfer Kredit dalam dokumentasi SNAP BI.

Berikut adalah beberapa API yang disediakan, yaitu:

1. SNAP Keamanan (Authorization)
2. API Account Inquiry - Customer Top Up (Inquiry)
3. API Customer Top Up (Payment)
4. API Customer Top Up Inquiry Status (Check Status)

`Update terakhir : 30 Mei 2024`

## Quick Start

Pada bagian ini dijelaskan mengenai tahapan integrasi dengan Merchant mulai dari Merchant mengajukan kerjasama hingga produk sudah bisa *live* di *production*. 

![img](./assets/img_flow_integrasi_merchant_topup-6163fd36.png)
Keterangan: 

**1.** Merchant yang ingin terintegrasi dengan AstraPay perlu melakukan pengajuan terlebih dahulu dengan partnership AstraPay untuk dapat mengetahui apa saja yang diperlukan untuk dapat memenuhi requirements yang dibutuhkan. 


**2.** Merchant menyiapkan dokumen yang diminta oleh partnership AstraPay dan mengirimkannya untuk dapat didaftarkan. 


**3.** Dokumen yang dikirim oleh Merchant perlu di verifikasi  oleh partnership AstraPay agar dapat memenuhi requirements pendaftaran. 


**4.** Tim partnership AstraPay akan berkoordinasi dengan tim developer AstraPay untuk dapat memberi panduan integrasi. 


**5.** Merchant melakukan development untuk integrasi sesuai dengan panduan yang diberikan AstraPay. 


**6.** Merchant perlu menginfokan pihak AstraPay apabila telah menyelesaikan proses development untuk integrasi. 


**7.** Pihak AstraPay akan mengirimkan dokumen terkait proses integrasi yang perlu dilengkapi oleh Merchant sebagai alat dokumentasi. 


**8.** Dokumen yang telah dilengkapi oleh Merchant akan dilakukan pengecekan oleh tim AstraPay untuk memastikan proses integrasi telah dilakukan oleh tim Merchant sesuai dengan panduan yang telah dibuat. 


**9.** Tim AstraPay akan menginfokan terkait kelanjutan proses integrasi untuk environment production & akses dashboard yang akan diberikan kepada Merchant. 


**10.** Setelah menyelesaikan proses integrasi, Merchant perlu menentukan bersama pihak AstraPay terkait jadwal live production. 


**11.** Aplikasi live production sesuai kesepakatan kedua pihak.

> [!NOTE]
> **Apa itu Uji Dev Site Pengguna & Uji Fungsionalitas? **
> 
> 
> Uji Dev Site Pengguna : Tahap dimana merchant melakukan pengujian pada Portal ASPI. Merchant memastikan bahwa fungsi dan API yang dibuat dapat berjalan pada portal ASPI sesuai dengan yang diharapkan.
> Uji Fungsionalitas : Tahap dimana merchant melakukan uji fungsionalitas untuk API yang sudah mereka develop, menggunakan curl yang sesuai dengan panduan yang diberikan.

## Environment


| Item | Value |
| --- | --- |
| Development | https://sandbox.astrapay.com |
| Production | URL production akan diinfokan setelah UAT selesai dilakukan |


## Tahap Integrasi Development

Dibawah ini adalah hal yang perlu disiapkan dan diketahui sebelum melakukan development untuk melakukan integrasi. Berikut persiapan credential yang diperlukan untuk komunikasi antar penyedia (AstraPay) dan pengguna (Merchant/Partner):

1. **Client ID (X-Client-Key)**, dibuat oleh penyedia dan diberikan kepada pengguna. Dibutuhkan untuk menandakan Merchant yang mengirim request.
2. **Client Secret**, dibuat oleh penyedia dan diberikan kepada pengguna. Dibutuhkan untuk menandakan Merchant yang mengirim request.
3. **Public Key**, dibuat oleh pengguna dan diberikan kepada penyedia.
4. **Private Key**, dibuat oleh pengguna dan disimpan oleh pengguna sendiri.
5. API yang membutuhkan Signature Auth, Signature Service, Token B2B, dan Token B2B2C sesuai pada sequence diagram, implementasinya dapat dilihat [disini](#snap-keamanan).

## SNAP Keamanan (Authorization)

Klik [disini](#snap-keamanan) untuk detail informasi SNAP Keamanan AstraPay.

## Penggunaan

Pada bagian ini menjelaskan mengenai penggunaan dari API Customer Top Up.

#### Use Case Diagram

Berikut adalah use case diagram untuk menggambarkan flow service Customer Top Up:
![img](./assets/img_top_up_use_case-2ef64524.jpg)

## API Account Inquiry - Customer Top Up

![img](./assets/img_top_up_seq_account_inquiry-e471331c.jpg)


| Item | Value |
| --- | --- |
| Name | Account Inquiry - Customer Top Up |
| Description | Request yang dikirim oleh Partner untuk memperoleh informasi customer |
| URI | [hostname]/disbursement-service/snap/v1.0/emoney/account-inquiry |
| Transport Protocol / HTTP Method | HTTPS / POST |
| Message Format | JSON |
| Service Code | 37 |


```shell
Authorization : Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a
X-SIGNATURE : 010cb949c2d087859b1d5ce96cbcc2be599d38c98da227dce6385792868cbdcb
X-TIMESTAMP : 2023-09-20T17:00:00.939
X-PARTNER-ID : 19e51b86-c5ae-4994-8a68-5ad251e86bac
X-EXTERNAL-ID  : 2023092000000001
CHANNEL-ID : 01437
```


| Parameter | Requirement | Description |
| --- | --- | --- |
| Authorization | Mandatory | Access token yang diberikan oleh AstraPay ketika partner melakukan generate token. |
| X-SIGNATURE | Mandatory | Signature yang dibuat oleh partner. |
| X-TIMESTAMP | Mandatory | Merupakan timestamp user melakukan topup dengan menggunakan format ISO8601. |
| X-PARTNER-ID | Mandatory | Merupakan kode partner yang diberikan oleh AstraPay. |
| X-EXTERNAL-ID | Mandatory | Kode transaksi partner yang bersifat unik setiap harinya. |
| CHANNEL-ID | Mandatory | ID dari service yang mengakses API Account Inquiry - Customer Top Up (01437). |
| X-LATITUDE | Optional | Kode latitude darimana request berasal. |
| X-LONGITUDE | Optional | Kode longitude darimana request berasal. |


```json
{
    "partnerReferenceNo": "23092915000075679102", 
    "customerNumber": "087878878878",
    "amount": {
        "value": "11000.00",
        "currency": "IDR"
    },
    "transactionDate": "2023-09-29T15:00:00+07:00",
    "additionalInfo": {
      "channelCode" : "APBANK"
    }
}
```


| Parameter | Type | Length | Requirement | Description |
| --- | --- | --- | --- | --- |
| partnerReferenceNo | String | 64 | Mandatory | Kode Identifikasi yang dikirimkan oleh partner / pengguna API.  Kode ini digunakan sebagai kode unik untuk melakukan payment. |
| customerNumber | String | 32 | Mandatory | Nomor HP Customer |
| amount | Object |  | Mandatory | Informasi terkait nominal yang akan dibayarkan ketika melakukan top up. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.   ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| transactionDate | String | 25 | Optional | Tanggal transaksi dilakukan dengan format ISO-8601. |
| additionalInfo | Object |  | Optional | Informasi tambahan jika ada. |
| channelCode | String |  | Conditional | Kode Channel Partner yang didaftarkan oleh AstraPay. Diperlukan ketika partner ingin mengidentifikasi transaksi berdasarkan Channel/Bank yang tercatat di AstraPay. |


```json
{
  "responseCode":"2003700",
  "responseMessage":"Successful",
  "referenceNo":"24032813211419996331",
  "partnerReferenceNo":"23092915000075679102",
  "customerNumber":"087878878878",
  "customerName":"ASTRAPAY CUSTOMER",
  "amount":{
    "value":"11000.00",
    "currency":"IDR"
  },
  "feeAmount":{
    "value":"1000.00",
    "currency":"IDR"
  },
  "minAmount":{
    "value":"10000.00",
    "currency":"IDR"
  },
  "maxAmount":{
    "value":"10000000.00",
    "currency":"IDR"
  },
  "customerMonthlyInLimit": 40000000.00,
  "feeType":"Admin Fee",
  "additionalInfo":{  
  } 
}
```


| Parameter | Type | Length | Requirement | Description |
| --- | --- | --- | --- | --- |
| responseCode | String | 7 | Mandatory | Response Code, untuk deskripsi dapat dilihat pada tabel response code di bawah. |
| responseMessage | String | 150 | Mandatory | Merupakan deskripsi dari response code yang diberikan. |
| referenceNo | String | 64 | Optional | Kode identifikasi transaksi yang akan diberikan Astrapay. |
| partnerReferenceNo | String | 64 | Mandatory | Kode identifikasi yang dikirimkan oleh partner / pengguna, akan dikembalikan jika partner / pengguna mengirimkan valuenya ketika request.  Kode ini digunakan sebagai kode unik untuk melakukan payment. |
| sessionId | String | 25 | Optional | Session Id untuk transaksi tersebut |
| customerNumber | Sting | 64 | Mandatory | Informasi Nomor HP Customer yang terdaftar. |
| customerName | Sting | 255 | Mandatory | Informasi nama customer, untuk saat ini akan mengembalikan ASTRAPAY CUSTOMER. |
| amount | Object |  | Optional | Informasi terkait nominal yang akan dibayarkan ketika melakukan top up. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.  ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| feeAmount | Object |  | Optional | Informasi terkait biaya administrasi yang akan dikenakan kepada customer untuk setiap transaksi top up yang dilakukannya. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.   ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| minAmount | Object |  | Optional | Informasi terkait limit minimal top up customer. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.  ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| maxAmount | Object |  | Optional | Informasi terkait limit maksimal top up yang dapat dilakukan terhadap seorang customer. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.  ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| customerMonthlyInLimit | Numeric | 17 | Optional | Informasi terkait limit maksimal customer melakukan top up setiap bulan. |
| feeType | String | 25 | Optional | Tipe fee yang dikenakan |
| additionalInfo | Object |  | Optional | Informasi Tambahan yang dibutuhkan partner / pengguna API |


## API Customer Top Up

![img](./assets/img_top_up_seq_customer_top_up-d95a9d99.jpg)


| Item | Value |
| --- | --- |
| Name | Customer Top-Up |
| Description | Request yang dikirim oleh Partner untuk melakukan top up saldo customer |
| URI | [hostname]/disbursement-service/snap/v1.0/emoney/topup |
| Transport Protocol / HTTP Method | HTTPS / POST |
| Message Format | JSON |
| Service Code | 38 |


```shell
Authorization : Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a
X-SIGNATURE : 010cb949c2d087859b1d5ce96cbcc2be599d38c98da227dce6385792868cbdcb
X-TIMESTAMP : 2023-09-20T17:00:00.939
X-PARTNER-ID : 19e51b86-c5ae-4994-8a68-5ad251e86bac
X-EXTERNAL-ID  : 2023092000000002
CHANNEL-ID : 01538
```


| Parameter | Requirement | Description |
| --- | --- | --- |
| Authorization | Mandatory | Access token yang diberikan oleh AstraPay ketika partner melakukan generate token. |
| X-SIGNATURE | Mandatory | Signature yang dibuat oleh partner. |
| X-TIMESTAMP | Mandatory | Merupakan timestamp user melakukan topup dengan menggunakan format ISO8601. |
| X-PARTNER-ID | Mandatory | Merupakan kode partner yang diberikan oleh AstraPay. |
| X-EXTERNAL-ID | Mandatory | Kode transaksi partner yang bersifat unik setiap harinya. |
| CHANNEL-ID | Mandatory | ID dari service yang mengakses API Customer Top Up (01538). |
| X-LATITUDE | Optional | Kode latitude darimana request berasal. |
| X-LONGITUDE | Optional | Kode longitude darimana request berasal. |


```json
{
  "partnerReferenceNo":"23092915000075679102",
  "customerNumber":"087878878878",
  "customerName":"ASTRAPAY CUSTOMER",
  "amount":{
    "value":"11000.00",
    "currency":"IDR"
  },
  "feeAmount":{
    "value":"1000.00",
    "currency":"IDR"
  },
  "transactionDate":"2023-09-29T15:00:59+07:00", 
  "notes":"notes test", 
  "additionalInfo":{ 
    "channelCode": "APBANK"
  } 
}
```


| Parameter | Type | Length | Requirement | Description |
| --- | --- | --- | --- | --- |
| partnerReferenceNo | String | 64 | Mandatory | Kode Identifikasi yang dikirimkan oleh partner / pengguna API.  Kode yang dikirimkan adalah kode unik yang didapatkan setelah partner berhasil melakukan inquiry. |
| customerNumber | String | 32 | Mandatory | Infomasi Nomor HP Customer yang terdaftar. |
| customerName | String | 255 | Optional | Informasi nama customer, untuk saat ini akan berisi ASTRAPAY CUSTOMER. |
| amount | Object |  | Mandatory | Informasi terkait nominal yang akan dibayarkan ketika  melakukan top up.  Object harus sama dengan response amount yang diberikan saat inquiry. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.   ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| feeAmount | Object |  | Mandatory | Informasi terkait biaya administrasi yang akan dikenakan kepada customer untuk setiap transaksi top up yang dilakukannya.  Object harus sama dengan response feeAmount yang diberikan saat inquiry. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.   ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| transactionDate | String | 25 | Optional | Tanggal transaksi dilakukan dengan format ISO-8601 |
| sessionId | String | 25 | Optional | Session Id untuk transaksi tersebut |
| categoryId | Numeric | 10 | Optional | - |
| notes | String | 255 | Optional | Informasi terkait notes transaksi tersebut. |
| additionalInfo | Object |  | Optional | Informasi tambahan jika ada. |
| channelCode | String |  | Conditional | Kode Channel Partner yang didaftarkan oleh AstraPay. Diperlukan ketika partner ingin mengidentifikasi transaksi berdasarkan Channel/Bank yang tercatat di AstraPay. |


```json
{
  "responseCode":"2003800",
  "responseMessage":"Successful",
  "referenceNo":"24032813211419996331",
  "partnerReferenceNo":"23092915000075679102",
  "customerNumber":"087878878878",
  "amount":{
    "value":"10000.00",
    "currency":"IDR"
  },
  "additionalInfo":{
  }
}
```


| Parameter | Type | Length | Requirement | Description |
| --- | --- | --- | --- | --- |
| responseCode | String | 7 | Mandatory | Response Code, untuk Deskripsi dapat dilihat pada tabel response code di bawah. |
| responseMessage | String | 150 | Mandatory | Merupakan deskripsi dari response code yang diberikan. |
| referenceNo | String | 64 | Optional | Kode identifikasi transaksi yang akan diberikan astrapay. |
| partnerReferenceNo | String | 64 | Mandatory | Kode identifikasi yang dikirimkan oleh partner / pengguna, akan dikembalikan jika partner / pengguna mengirimkan valuenya ketika request. |
| sessionId | String | 25 | Optional | Session Id untuk transaksi tersebut |
| customerNumber | Sting | 64 | Mandatory | Informasi Nomor HP Customer yang terdaftar. |
| amount | Object |  | Mandatory | Informasi terkait saldo yang akan diterima oleh customer ketika melakukan top up. |
| value | String | 16,2 | Mandatory | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.   ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Mandatory | ex : IDR |
| additionalInfo | Object |  | Optional | Informasi tambahan yang dibutuhkan partner / pengguna API |


## API Customer Top Up Inquiry Status

![img](./assets/img_top_up_seq_inquiry_status-71376550.jpg)


| Item | Value |
| --- | --- |
| Name | Customer Top Up Inquiry Status |
| Description | Request yang dikirim oleh Partner untuk melakukan check status top up saldo customer |
| URI | [hostname]/disbursement-service/snap/v1.0/emoney/topup-status |
| Transport Protocol / HTTP Method | HTTPS / POST |
| Message Format | JSON |
| Service Code | 39 |


```shell
Authorization : Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a
X-SIGNATURE : 010cb949c2d087859b1d5ce96cbcc2be599d38c98da227dce6385792868cbdcb
X-TIMESTAMP : 2023-09-20T17:00:00.939
X-PARTNER-ID : 19e51b86-c5ae-4994-8a68-5ad251e86bac
X-EXTERNAL-ID  : 2023092000000003
CHANNEL-ID : 01639
```


| Parameter | Requirement | Description |
| --- | --- | --- |
| Authorization | Mandatory | Access token yang diberikan oleh AstraPay ketika partner melakukan generate token. |
| X-SIGNATURE | Mandatory | Signature yang dibuat oleh partner. |
| X-TIMESTAMP | Mandatory | Merupakan timestamp user melakukan topup dengan menggunakan format ISO8601. |
| X-PARTNER-ID | Mandatory | Merupakan kode partner yang diberikan oleh AstraPay. |
| X-EXTERNAL-ID | Mandatory | Kode transaksi partner yang bersifat unik setiap harinya. |
| CHANNEL-ID | Mandatory | ID dari service yang mengakses API Customer Top Up Inquiry Status (01639). |
| X-LATITUDE | Optional | Kode latitude darimana request berasal. |
| X-LONGITUDE | Optional | Kode longitude darimana request berasal. |


```json
{
  "originalPartnerReferenceNo":"23092915000075679102",
  "serviceCode":"38",
  "additionalInfo":{ 
  } 
}
```


| Parameter | Type | Length | Requirement | Description |
| --- | --- | --- | --- | --- |
| originalPartnerReferenceNo | String | 64 | Mandatory | Kode Identifikasi yang dikirimkan oleh partner / pengguna API pada request inquiry atau payment. |
| originalReferenceNo | String | 64 | Optional | Kode Identifikasi yang diterima oleh partner / pengguna API pada saat melakukan inquiry atau payment. |
| originalExternalId | String | 64 | Optional | Header X-EXTERNAL-ID yang dikirim oleh partner / pengguna API ketika melakukan inquiry atau payment. |
| serviceCode | String | 2 | Mandatory | Service code yang ingin dilakukan pengecekan, dalam kasus ini maka:  38 → Pengecekan request payment |
| additionalInfo | Object |  | Optional | Informasi Tambahan dari Partner / Pengguna API |


```json
{
  "responseCode":"2003900",
  "responseMessage":"Successful",
  "originalPartnerReferenceNo":"23092915000075679102",
  "originalReferenceNo":"24032813211419996331",
  "serviceCode":"38",
  "amount":{
    "value":"10000.00",
    "currency":"IDR"
  },
  "latestTransactionStatus":"00",
  "additionalInfo":{
  } 
}
```


| Parameter | Type | Length | Requirement | Description |
| --- | --- | --- | --- | --- |
| responseCode | String | 7 | Mandatory | Response Code, untuk Deskripsi dapat dilihat pada tabel response code dibawah. |
| responseMessage | String | 150 | Mandatory | Merupakan deskripsi dari response code yang diberikan. |
| originalPartnerReferenceNo | String | 64 | Mandatory | Kode Identifikasi yang dikirimkan oleh partner / pengguna API pada request inquiry atau payment. |
| originalReferenceNo | String | 64 | Optional | Kode Identifikasi yang diterima oleh partner / pengguna API pada saat melakukan inquiry atau payment. |
| originalExternalId | String | 64 | Optional | Header X-EXTERNAL-ID yang dikirim oleh partner / pengguna API ketika melakukan inquiry atau payment. |
| serviceCode | String | 2 | Mandatory | Service code yang ingin dilakukan pengecekan, dalam kasus ini maka :  38 → Pengecekan request payment |
| amount | Object |  | Optional | Infromasi terkait saldo yang akan diterima oleh customer ketika melakukan topup. |
| value | String | 16,2 | Optional | Value transaksi, jika currency dalam IDR maka tambahkan 2 digit desimal.   ex : IDR10.000 → 10000.00 |
| currency | String | 3 | Optional | ex : IDR |
| latestTransactionStatus | String | 2 | Mandatory | Adapun code transaction status yang akan dikembalikan oleh Astrapay adalah sebagai berikut:  00 → Success 01 → Initiated 03 → Pending 05 → Cancelled 06 → Failed 07 → Not Found |
| transactionStatusDesc | String | 50 | Optional | Deskripsi atau notes terkait status transaksi. |
| additionalInfo | Object |  | Optional | Informasi tambahan jika ada |


## Response Code

Response status terdiri dari 2 komponen, yaitu kode (response code) dan deskripsinya (response message).

Daftar Response Code


| HTTP Code | Service Code | Case Code | Response Message | Description |
| --- | --- | --- | --- | --- |
| 200 | any | 00 | Successful | Successful request. |
| 202 | any | 00 | Request In Progress | Transaction still on process |
| 400 | any | 00 | Bad Request | General request failed error, including message parsing failed. |
| 400 | any | 01 | Invalid Field Format {field name} | Invalid format. |
| 400 | any | 02 | Invalid Mandatory Field {field name} | Missing or invalid format on mandatory field. |
| 401 | any | 00 | Unauthorized. [reason] | General unauthorized error (No Interface Def, API is Invalid, Oauth Failed, Verify Client Secret Fail, Client Forbidden Access API, Unknown Client, Key not Found). |
| 401 | any | 01 | Invalid Token (B2B) | Token found in request is invalid (Access Token Not Exist, Access Token Expiry). |
| 403 | any | 15 | Transaction Not Permitted.[reason] | Transaction Not Permitted. |
| 403 | any | 18 | Inactive Card/Account/Customer | Indicates inactive account. |
| 404 | any | 01 | Transaction Not Found | Transaction Not Found |
| 404 | any | 08 | Invalid Merchant | Merchant does not exist or status abnormal. |
| 404 | any | 11 | Invalid Card/Account/Customer [info]/Virtual Account | Card information may be invalid, or the card account may be blacklisted, or Virtual Account number maybe invalid. |
| 404 | any | 12 | Invalid Bill/Virtual Account [Reason] | The bill is blocked/ suspended/not found. |
| 404 | any | 13 | Invalid Amount | The amount doesn't match with what supposed to. |
| 404 | any | 14 | Paid Bill | The bill has been paid |
| 404 | any | 16 | Partner Not Found | Partner number can't be found. |
| 404 | any | 19 | Invalid Bill/Virtual Account | The bill is expired. |
| 409 | any | 00 | Conflict | Cannot use same X-EXTERNAL-ID in same day. |
| 409 | any | 01 | Duplicate partnerReferenceNo | Transaction has previously been processed indicates the same partnerReferenceNo already success. |
| 500 | any | 00 | General Error | General Error. |
| 500 | any | 01 | Internal Server Error | Unknown Internal Server Failure, Please retry the process again. |
| 504 | any | 00 | Timeout | Timeout from the issuer. |

