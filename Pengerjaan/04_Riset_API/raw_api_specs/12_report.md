# Report


## 1. Introduction Report

Selamat datang di dokumentasi AstraPay Report.

Dokumentasi ini menjelaskan  *procedure acceptance*  untuk implementasi *API External Report* dari perspektif Merchant.

Saat ini, AstraPay Reporting menyediakan metode pengambilan Report via API AstraPay. Berikut adalah beberapa hal yang dijelaskan pada dokumen ini:

1. Quick Start
2. Tahap Integrasi *Development*
3. Environment & Testing
4. SNAP Keamanan
5. External Report


    - API Get Available Report
    - API Generate Report By ID
6. Rate Limiting
7. Troubleshooting & FAQ

`Update terakhir : 22 April 2026`

## 2. Quick Start

Pada bagian ini dijelaskan mengenai tahapan mulai dari Merchant mengajukan kerjasama hingga report sudah bisa digunakan *live* di *production*. 


![img](./assets/img_flow_integrasi_report_external-d5efa411.png) 

Keterangan: 

**1.** Merchant/Partner mengajukan permintaan Report API kepada Tim AstraPay. 

**2.** Merchant/Partner mengirimkan requirement Report (beserta contoh file apabila memungkinkan). 

**3.** Tim Report AstraPay melakukan pengecekan data sesuai requirement yang diberikan Merchant/Partner. 

**4.** Tim Report AstraPay mengirimkan contoh hasil Report yang telah dikerjakan kepada Merchant/Partner. 

**5.** Merchant/Partner melakukan pengecekan contoh hasil Report yang dikirimkan Tim Report AstraPay. 

**6.** Jika sudah sesuai, Tim Report AstraPay akan memberikan ID kepada Merchant/Partner, Jika belum sesuai, Tim Report AstraPay akan kembali melakukan pengecekan data dan mengirimkan kembali hasil report yang telah disesuaikan. 

**7.** Merchant/Partner sudah bisa generate Report menggunakan ID yang diberikan.

## 3. Tahap Integrasi Development

Di bawah ini adalah hal yang perlu disiapkan dan diketahui sebelum melakukan *development* untuk melakukan integrasi. Berikut persiapan *credential* yang diperlukan untuk komunikasi antar penyedia (AstraPay) dan pengguna (Merchant/Partner):

1. **Client ID (X-Client-Key)**, dibuat oleh penyedia dan diberikan kepada pengguna. Dibutuhkan untuk menandakan Merchant yang mengirim request.
2. **Client Secret**, dibuat oleh penyedia dan diberikan kepada pengguna. Dibutuhkan untuk menandakan Merchant yang mengirim request.
3. **Public Key**, dibuat oleh pengguna dan diberikan kepada penyedia.
4. **Private Key**, dibuat oleh pengguna dan disimpan oleh pengguna sendiri.
5. API yang membutuhkan Signature Auth, Signature Service, Token B2B, dan Token B2B2C sesuai pada sequence diagram, implementasinya dapat dilihat [disini](#snap-keamanan).

## 4. Environment & Testing

### 4.1 Endpoint Environment


| Environment | Hostname | Keterangan |
| --- | --- | --- |
| Development | https://sandbox.astrapay.com | Lingkungan sandbox untuk testing & development |
| Production | URL production akan diinformasikan | Lingkungan production untuk live transaction |


### 4.2 Testing & Development

Untuk melakukan testing di environment development, gunakan seluruh endpoint dengan mengganti hostname ke `https://sandbox.astrapay.com`. Semua credential dan flow yang digunakan sama seperti production, hanya hostname dan data yang berbeda.

Contoh:
- Development: `https://sandbox.astrapay.com/report-service/api/ext`
- Production: URL production akan diinformasikan

## 5. SNAP Keamanan

Klik [disini](#snap-keamanan) untuk detail informasi SNAP Keamanan AstraPay.

## 6. External Report API

### 6.1 External Report API - Get Available Reports

![img](./assets/img_sequence_report_external_get_all-f621ce2d.png)

**Get Available Reports - Complete Code**

```shell
curl -X GET https://sandbox.astrapay.com/report-service/api/ext \
  -H "Authorization: Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a" \
  -H "X-SIGNATURE: 010cb949c2d087859b1d5ce96cbcc2be599d38c98da227dce6385792868cbdcb" \
  -H "X-TIMESTAMP: 2024-10-10T10:00:00.101" \
  -H "X-PARTNER-ID: 19e51b86-c5ae-4994-8a68-5ad251e86bac" \
  -H "X-EXTERNAL-ID: 2023092000000001"
```

Endpoint untuk API Get Available Reports memiliki URL seperti di bawah ini:

Protocol: HTTPS


Method: GET


URL Sandbox: https://sandbox.astrapay.com/report-service/api/ext

**Rate Limiting:** 30 request per menit per User ID / Token



### Request Header

**Get Available Reports - Sample Request Header**

```shell
Authorization : Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a
X-SIGNATURE : 010cb949c2d087859b1d5ce96cbcc2be599d38c98da227dce6385792868cbdcb
X-TIMESTAMP : 2024-10-10T10:00:00.101
X-PARTNER-ID : 19e51b86-c5ae-4994-8a68-5ad251e86bac
X-EXTERNAL-ID  : 2023092000000001
```

Request Header yang harus dimasukkan untuk hit API Get Available Reports adalah sebagai berikut:


| Parameter | Requirement | Description |
| --- | --- | --- |
| Authorization | Mandatory | Access token yang diberikan oleh AstraPay ketika partner melakukan generate token. |
| X-SIGNATURE | Mandatory | Signature yang dibuat oleh partner. |
| X-TIMESTAMP | Mandatory | Merupakan timestamp user melakukan request dengan menggunakan format ISO8601. |
| X-PARTNER-ID | Mandatory | Merupakan kode partner yang diberikan oleh AstraPay. |
| X-EXTERNAL-ID | Mandatory | Kode transaksi partner yang bersifat unik setiap harinya. |


### Response Body

**Get Available Report - Sample Response Body**

```json
[
  {
    "id": 1,
    "reportName": "Transaksi Sukses"
  },
  {
    "id": 2,
    "reportName": "Transaksi Gagal"
  }
]
```

Setelah berhasil melakukan request, maka partner akan mendapatkan respon sebagai berikut:


| Parameter | Description |
| --- | --- |
| id | ID Report |
| reportName | Nama Template Report |


**Get Available Reports - Sample Response Body (Unauthorized)**

```json
{
  "responseCode": "401",
  "responseMessage": "Invalid Token (B2B)"
}
```

### 6.2 External Report API - Generate Report By ID

![img](./assets/img_sequence_report_external_generate-dde4edec.png)

**Generate Report By ID - Complete Code**

```shell
curl --request GET \
  --url 'hostname/report-service/api/ext/3?startDate=2026-04-13T00:00:00&endDate=2026-04-14T00:00:00' \
  -H "Authorization: Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a" \
  -H "X-SIGNATURE: 010cb949c2d087859b1d5ce96cbcc2be599d38c98da227dce6385792868cbdcb" \
  -H "X-TIMESTAMP: 2024-10-10T10:00:00.939" \
  -H "X-PARTNER-ID: 19e51b86-c5ae-4994-8a68-5ad251e86bac" \
  -H "X-EXTERNAL-ID: 2023092000000002" \
  -H "Accept: text/csv"
```

Endpoint untuk API Generate Report By ID memiliki URL seperti di bawah ini:

Protocol: HTTPS


Method: GET


URL Sandbox: hostname/report-service/api/ext/[id]?startDate=[startDate]&endDate=[endDate]

**Rate Limiting:** 5 request per menit per User ID / Token



### Request Header

**Generate Report By ID - Sample Request Header**

```shell
Authorization : Bearer gp9HjjEj813Y9JGoqwOeOPWbnt4CUpvIJbU1mMU4a11MNDZ7Sg5u9a
X-SIGNATURE : 010cb949c2d087859b1d5ce96cbcc2be599d38c98da227dce6385792868cbdcb
X-TIMESTAMP : 2024-10-10T10:00:00.939
X-PARTNER-ID : 19e51b86-c5ae-4994-8a68-5ad251e86bac
X-EXTERNAL-ID  : 2023092000000002
Accept : text/csv
```

Request Header yang harus dimasukkan untuk hit API Generate Report By ID adalah sebagai berikut:


| Parameter | Requirement | Description |
| --- | --- | --- |
| Authorization | Mandatory | Access token yang diberikan oleh AstraPay ketika partner melakukan generate token. |
| X-SIGNATURE | Mandatory | Signature yang dibuat oleh partner. |
| X-TIMESTAMP | Mandatory | Merupakan timestamp user melakukan request dengan menggunakan format ISO8601. |
| X-PARTNER-ID | Mandatory | Merupakan kode partner yang diberikan oleh AstraPay. |
| X-EXTERNAL-ID | Mandatory | Kode transaksi partner yang bersifat unik setiap harinya. |
| Accept | Mandatory | Tipe File Report: text/csv → CSV text/plain → TXT application/vnd.openxmlformats-officedocument.spreadsheetml.sheet → XLSX |


### Query Parameter

**Generate Report By ID - Sample Query Parameter**

```shell
startDate=2026-04-13T00:00:00
endDate=2026-04-14T00:00:00
```

Query parameter yang harus dimasukkan adalah sebagai berikut:


| Parameter | Type | Length | Requirement | Description |
| --- | --- | --- | --- | --- |
| startDate | Datetime | 19 | Mandatory | Waktu batas awal data (format: yyyy-MM-ddTHH:mm:ss) |
| endDate | Datetime | 19 | Mandatory | Waktu batas akhir data (format: yyyy-MM-ddTHH:mm:ss) |


> [!NOTE]
> Query parameter `startDate` dan `endDate` menggunakan format `yyyy-MM-ddTHH:mm:ss` (contoh: `2026-04-13T00:00:00`), sedangkan header X-TIMESTAMP menggunakan format ISO8601 (contoh: `2024-10-10T10:00:00.101`). Pastikan Anda menggunakan format yang sesuai untuk setiap field.

### Response Header

**Generate Report By ID - Sample Response Header**

```shell
Content-Disposition : attachment; filename="[nama_file_report].[tipe_file_sesuai_header_accept]"
Content-Type : [tipe_file_sesuai_header_accept]
```

Setelah berhasil melakukan request, file report akan dikirimkan dengan response header sebagai berikut:


| Parameter | Description |
| --- | --- |
| Content-Disposition | Nama File Report |
| Content-Type | Tipe File Report |


**Generate Report By ID - Sample Response Body (Success - CSV)**

```text
transaction_id,merchant_id,amount,status,date,reference_number
TRX001,MERCH123,50000,SUCCESS,2024-10-01 10:30:00,REF001
TRX002,MERCH123,75000,SUCCESS,2024-10-02 14:15:00,REF002
TRX003,MERCH123,100000,SUCCESS,2024-10-03 09:45:00,REF003
```

**Generate Report By ID - Sample Response Body (Report Not Found)**

```json
{
  "responseCode": "404",
  "responseMessage": "Report ID 1 not found"
}
```

**Generate Report By ID - Sample Response Body (Invalid Format)**

```json
{
  "responseCode": "400",
  "responseMessage": "Validation failed: start date must not be empty"
}
```

### 6.3 Response Code

Response status terdiri dari 2 komponen, yaitu kode (response code) dan deskripsinya (response message).

Daftar Response Code


| HTTP Code | Response Message | Description |
| --- | --- | --- |
| 200 | Successful | Successful request. |
| 400 | Bad Request | General request failed error, including message parsing failed. |
| 400 | Invalid Field Format {field name} | Invalid format pada field yang disebutkan. |
| 400 | Invalid Mandatory Field {field name} | Missing atau invalid format pada mandatory field. |
| 401 | Unauthorized. [reason] | General unauthorized error (No Interface Def, API is Invalid, Oauth Failed, Verify Client Secret Fail, Client Forbidden Access API, Unknown Client, Key not Found). |
| 401 | Invalid Token (B2B) | Token yang dikirim invalid (Access Token Not Exist, Access Token Expiry). |
| 404 | Report Not Found | Report dengan ID yang diminta tidak ditemukan. |
| 429 | Too Many Requests | Rate limit terlampaui. Tunggu sebelum mengirim request berikutnya. |
| 500 | General Error | General Error. |
| 500 | Internal Server Error | Unknown Internal Server Failure, Please retry the process again. |
| 504 | Timeout | Timeout dari issuer. |


## 7. Rate Limiting

Setiap endpoint memiliki rate limit yang berbeda-beda untuk memastikan stabilitas layanan. Rate limit dihitung per User ID atau per Token yang digunakan.


| Endpoint | Limit | Window | Deskripsi |
| --- | --- | --- | --- |
| Get Available Reports | 30 request | 1 menit | Maksimal 30 request per menit per User ID/Token |
| Generate Report By ID | 5 request | 1 menit | Maksimal 5 request per menit per User ID/Token |


Ketika rate limit terlampaui, server akan mengembalikan HTTP 429 (Too Many Requests):

**Sample Response Body (Too Many Requests)**

```json
{
  "responseCode": "429",
  "responseMessage": "Too many requests. Please retry after 30 seconds"
}
```

Implementasi retry dengan exponential backoff direkomendasikan. Tunggu beberapa detik sebelum melakukan retry berikutnya.

## 8. Troubleshooting & FAQ

Untuk pertanyaan umum dan troubleshooting, silakan lihat [FAQ section](#faq).

**Q: Apa perbedaan antara X-TIMESTAMP format di header dan date format di query parameter?**
A: Header X-TIMESTAMP harus menggunakan format ISO8601 (`yyyy-MM-ddTHH:mm:ss.SSS`), sedangkan parameter `startDate` dan `endDate` pada query string menggunakan format `yyyy-MM-ddTHH:mm:ss`. Pastikan Anda tidak mencampurkan kedua format ini.

**Q: Bagaimana menangani error "Invalid Token (B2B)"?**
A: Error ini terjadi ketika access token sudah expired atau tidak valid. Anda perlu men-generate token baru melalui proses autentikasi. Lihat [SNAP Keamanan](#snap-keamanan) untuk detail proses generate token.

**Q: Apa yang harus dilakukan jika rate limit terlampaui?**
A: Tunggu hingga window rate limit berakhir (1 menit) sebelum mengirim request berikutnya. Implementasikan exponential backoff retry logic seperti contoh di atas untuk handling yang lebih baik.

**Q: Bagaimana cara men-generate X-SIGNATURE?**
A: Signature dibuat berdasarkan client secret dan data request. Lihat [SNAP Keamanan](#snap-keamanan) untuk detail lengkap tentang cara generate signature.

Untuk pertanyaan lebih lanjut, hubungi tim support AstraPay atau lihat [FAQ documentation](#faq).
