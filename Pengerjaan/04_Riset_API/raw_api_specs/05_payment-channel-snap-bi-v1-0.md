# Payment Channel SNAP BI V1.0


## SNAP Introduction

Saat ini, AstraPay menyediakan metode baru untuk calon merchant yang ingin melakukan integrasi dengan AstraPay, melalui SNAP AstraPay API. Dimana SNAP (Standar Nasional Open API Pembayaran) adalah standar Open API yang ditetapkan Bank Indonesia agar menciptakan industri sistem pembayaran yang lebih maju di Indonesia.

Untuk melakukan integrasi dengan AstraPay, merchant diharuskan untuk melakukan pendaftaran melalui link dibawah ini.

[
Daftarkan Merchant Saya
](./assets/registration_1)

**[Sudah terdaftar dan ingin melakukan penyesuaian ](#panduan-penyesuaian-snap)**

## Quick Start

Dibawah ini adalah hal yang perlu kamu ketahui untuk melakukan integrasi:
Berikut adalah alur integrasi yang akan dilalui Merchant: 

![img](./assets/img_flow_integrasi_merchant_V2-e4d6c6e6_1.png)
Keterangan: 

**1.** Alur integrasi dengan Astrapay menyesuaikan dengan kondisi Merchant, apabila Merchant belum pernah integrasi, dapat lanjut ke tahap **2A.** Apabila sudah integrasi, lanjut ke tahap **2B**. 


**2A.** Apabila Merchant belum pernah melakukan integrasi, Merchant dapat mengisi formulir pendaftaran pada [halaman ini.](./assets/registration_1)


**3A.** Pihak AstraPay akan memeriksa data registrasi yang sudah diisikan dan akan di *follow up* mengenai kelanjutan tahap integrasi. 


**2B.** Pihak AstraPay akan menghubungi Merchant yang sudah integrasi untuk membahas mengenai timeline penggunaan SNAP AstraPay. 


**3B.** Pihak AstraPay kemudian akan mengirimkan email untuk kelanjutan proses integrasi menggunakan API SNAP secara teknis.


**4.** Merchant dapat melanjutkan development untuk integrasi. Merchant yang sebelumnya sudah integrasi dapat melihat [**Panduan Migrasi Menggunakan API SNAP**](#panduan-penyesuaian-snap-astrapay).


**5.** Apabila development sudah selesai, Merchant diharapkan untuk menghubungi AstraPay.


**6.** Selanjutnya Merchant mengunduh dan mengisi semua dokumen testing yaitu Dev Site, Fungsionalitas, dan UAT yang tersedia di bawah ini.


**7.** Dokumen testing yang sudah diisi dikirimkan ke pihak AstraPay untuk dilakukan review. 


**8.** Setelah direview, pihak AstraPay akan menghubungi Merchant terkait kebutuhan *environment* Production & akses Dashboard Merchant. 


**9.** Apabila Merchant sudah siap untuk live ke Production, Merchant diharapkan memberi tahu Pihak AstraPay.


**10.** Aplikasi Merchant dapat live ke Production.

> [!NOTE]
> **Apa itu Uji Dev Site Pengguna, Uji Fungsionalitas, dan UAT?**
> 
> 
> Uji Dev Site Pengguna : Tahap dimana merchant melakukan pengujian pada Portal ASPI. Merchant memastikan bahwa fungsi dan API yang dibuat dapat berjalan pada portal ASPI sesuai dengan yang diharapkan.
> Uji Fungsionalitas : Tahap dimana merchant melakukan uji fungsionalitas untuk API yang sudah mereka develop, menggunakan curl yang sesuai dengan panduan yang diberikan. Merchant dapat mengunduh Template Uji Fungsionalitas disini.
> UAT (User Acceptance Test) : Merchant harus melakukan testing untuk pengujian user interface dan customer flow pada environment UAT. Merchant juga harus mengisi dokumen UAT sesuai dengan metode pembayaran yang digunakan. Template dokumen UAT dapat diunduh di sini: UAT- Payment with Linking / UAT - Push to Payment/ UAT - In-App Payment / UAT- Auth Payment.

## Environment


| Item | Value |
| --- | --- |
| Development | https://sandbox.astrapay.com |
| Production | URL production akan dikirimkan melalui email terdaftar setelah UAT selesai dilakukan |


## Tahap Integrasi Development

Dibawah ini adalah hal yang perlu disiapkan dan diketahui sebelum melakukan development untuk melakukan integrasi:

1. Mengisi **formulir pendaftaran URL Merchant Payment Channel** lalu mengirimkannya ke pihak AstraPay. Formulir URL dapat diunduh [disini](./assets/Formulir_Pendaftaran_URL_Merchant_Payment_Channel__Sandbox__1.docx).
2. Menyiapkan credential yang diperlukan untuk komunikasi antar penyedia (AstraPay) dan pengguna (Merchant/Partner):

1. **Client ID (X-Client-Key)**, dibuat oleh penyedia dan diberikan kepada pengguna. Dibutuhkan untuk menandakan Merchant yang mengirim request.
2. **Client Secret**, dibuat oleh penyedia dan diberikan kepada pengguna. Dibutuhkan untuk menandakan Merchant yang mengirim request.
3. **Public Key**, dibuat oleh pengguna dan diberikan kepada penyedia.
4. **Private Key**, dibuat oleh pengguna dan disimpan oleh pengguna sendiri.
5. API yang membutuhkan Signature Auth, Signature Service, Token B2B, dan Token B2B2C sesuai pada sequence diagram, implementasinya dapat dilihat [disini](#snap-keamanan).

## SNAP Registration

Layanan ini digunakan untuk mendaftarkan dan menghubungkan akun customer pada platform Merchant ke akun AstraPay customer.

### Use Case Diagram

Berikut adalah flow untuk proses *binding*:

![img](./assets/useCaseDiagramBinding-fefc0792_1.png)

### Sequence Diagram

Berikut adalah *sequence diagram* untuk proses *binding*:
![img](./assets/snapAccountBinding-9f19b82f_1.png)

Layanan ini terdiri dari 4 API, diantaranya:


| Nama API | Deskripsi |
| --- | --- |
| [**API Account Binding**](#api-account-binding) | Digunakan untuk mendapatkan authCode |
| [**API Access Token B2B2C**](#api-access-token-b2b2c) | Digunakan untuk mengambil token otoriasasi user sebagai identifikasi pada setiap API yang berkaitan dengan data customer |
| [**API Account Binding Inquiry (Opsional)**](#api-account-binding-inquiry) | Digunakan untuk mengecek status binding customer |
| [**API Account Unbinding**](#api-account-unbinding) | Digunakan untuk memutuskan binding antara akun pengguna layanan (Merchant) dengan akun penyedia layanan (AstraPay) |


### API Account Binding

API ini digunakan untuk mendapatkan authCode.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 07 |
| Channel ID | 01207 |
| Method | POST |
| URL Sandbox | /snap-service/snap/v1.0/registration-account-binding |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Account Binding (01207) |


### Request Body

**Contoh cURL Account Binding**

```shell
curl --location 'https://sandbox.astrapay.com/snap-service/snap/v1.0/registration-account-binding' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU95STd1R0g0dV9ZMzBmeTc4b1I1T2V6S3JIdVpzIn0.eyJleHAiOjE3MDY4Mzk3NTksImlhdCI6MTcwNjgwMzc1OSwianRpIjoiZmUyOTEyNzMtNTM1Ny00MmE1LWExODMtOWUxNWNiZTdjYjBmIiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay1zaXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwic3ViIjoiNDY2NTk5NzEtZTgwZC00OThiLTlkNjYtM2VhMTJjNDY5Mzk5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbImRlZmF1bHQtcm9sZXMtYXN0cmFwYXkiXX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudEhvc3QiOiIxODIuMjUzLjU5LjEwNSIsImNsaWVudElkIjoiYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwiY2xpZW50QWRkcmVzcyI6IjE4Mi4yNTMuNTkuMTA1In0.UhjrPznIeUiRMxptnrTeLN_Z6svcN0RzDsAU8oj_F965hRKVWDfp_B4kp2i1gnaC4Tp8X9jC-5xSEBicSZR4I-TitsuikPvT4l_NGqXcHCxSa4noeQmjoDysMILotTqa3rIx9_dWxvIyZil_t4b7L7fhTCVB-B6boxYpUGpBaNOxl6gdC8184AwFX-X8bqBTe2t9BaWZ6QBWAaarB4MdjYArrXW8N5C1oraPh4oAz2O2JVEhdM5ls1Jj0p2SlAVlsYSr9LbFJkwlJSJirDgjcArBbyFhx9OKLJT5F_7gvZm8iOBZuiUTOz4V0Sk79W-mwL0UcTIkCp1F-_PPfM0r3w' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2023-06-02T13:20:19+07:00' \
--header 'X-SIGNATURE: DMQt5kjGE40k+GayNs+tY1VYWoHfum6ASuiEUp51ituU0/yoA4ZhQGyMeRXjBkrbbYhRR+YB3aiIss7Y5b5p4w==' \
--header 'X-PARTNER-ID: 22fd3727-3044-4596-8552-f4e54205f540' \
--header 'X-EXTERNAL-ID: 9812839751201231223895' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'CHANNEL-ID: 01207' \
--data '{
    "merchantId":"9653109f-2806-48cf-ad75-85664c2db33e",
    "phoneNo": "081111111111",
    "additionalInfo": {
        "finishBindingUrl": "https://Merchant.com",
        "externalUid":"Testing22",
        "name":"John Doe",
        "email":"[email protected]"
    }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| phoneNo | String | Optional | Nomor HP customer, apabila field ini terisi maka user wajib login dengan nomor yang sudah disertakan |
| additionalInfo | Object | Mandatory | Informasi tambahan |
| additionalInfo.finishBindingUrl | String | Mandatory | URL yang digunakan sebagai callback setelah proses get authCode berhasil |
| additionalInfo.externalUid | String | Mandatory | ID milik user pada aplikasi partner (Merchant user ID) |
| additionalInfo.name | String | Optional | Nama lengkap customer pada platform merchant, apabila field ini terisi maka data akan otomatis terisi pada halaman registrasi dan tetap dapat diubah oleh user |
| additionalInfo.email | String | Optional | Email customer pada platform merchant, apabila field ini terisi maka data akan otomatis terisi pada halaman registrasi dan tetap dapat diubah oleh user |


### Response Body

**Contoh Response**

```shell
{
  "responseCode": "2000700",
  "responseMessage": "Successful",
  "referenceNo": "644efc4f-9bb1-45ef-a6cf-1e396cbf1075",
  "redirectUrl": "https://sandbox.astrapay.com/payment-channel/account-binding/644efc4f-9bb1-45ef-a6cf-1e396cbf1075",
  "additionalInfo": {
    "authCode": "nlleyvaQcB2SOkVjtF7ceCuC16BiqCc6DRrjaKOnnfRdToW8Tr"
  }
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| referenceNo | String | Mandatory | ID transaksi pada AstraPay |
| redirectUrl | String | Mandatory | URL yang digunakan untuk mengarahkan user ke webview AstraPay untuk menyelesaikan proses *binding account* |
| additionalInfo | Object | Mandatory | Informasi Tambahan |
| additionalInfo.authCode | String | Mandatory | Authorization berupa string yang diberikan untuk pengguna dan dapat digunakan untuk mendapatkan akses token B2B2C |


### API Access Token B2B2C

API ini digunakan untuk mengambil token otorisasi user sebagai identifikasi

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 74 |
| Method | POST |
| URL Sandbox | /snap-service/snap/v1.0/access-token/b2b2c |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk API keamanan B2B Access Token Request (Signature Auth). Verifikasi signature dapat dilakukan oleh penyedia dengan menggunakan public key yang diberikan oleh pengguna (Merchant/Partner) |
| X-CLIENT-KEY | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |


### Request Body

**Contoh cURL Access Token B2B2C**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/snap-service/snap/v1.0/access-token/b2b2c' \
--header 'X-TIMESTAMP: 2025-06-22T14:29:07+07:00' \
--header 'X-SIGNATURE: GsslIrqId/Lm7IZ+QrkJaFrxsZ+dAPB6ABKGysKGee8kY77Ffm73QLClWT8VKjelrF2v/d5C/l3154q1KooFnFlkQFB9FgnikYmILybJCsFT10M02HDpILyzk0P7Ibehwgta6H7Fb0eV95FpbMa76uPByNnYfwKEv+HVCa2mzsb1SW5tv7yy6j1ktTm5vCD++aTSK+klY42TAA3nlCa+d3bGjA+NXBqtpsb3iUJ/BpBZS+7ewFia08evxvtJgP5RFULkhym4uQ0bRgFrsfi2YJPuwPNf55fxmQSaPeeKbiyvmV3WrFykuCtPTevBuGYoW911TpTFCRXA2iwAs0eWGw==' \
--header 'X-CLIENT-KEY: 22fd3727-3044-4596-8552-f4e54205f540' \
--header 'Content-Type: application/json' \
--data-raw '{
    "grantType": "AUTHORIZATION_CODE",
    "authCode": "ZV2G6HALVQIRN906P1TU2YV17ISN2S5LZBOZ3XW1OCAMCKITWR"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| grantType | String | Mandatory | Penerapan tipe token request, bisa menggunakan AUTHORIZATION_CODE atau REFRESH_TOKEN |
| authCode | String | Conditional | Authorization code yang diterima setelah user memberikan persetujuan. Wajib apabila grantType = AUTHORIZATION_CODE |
| refreshToken | String | Conditional | Digunakan untuk mendapatkan accessToken baru dimana user tidak perlu memberikan persetujuan lagi. Bersifat wajib ketika grantType = REFRESH_TOKEN. Harus kurang dari validitas access token dan akan diatur oleh aplikasi penyedia untuk menghasilkan accessToken baru. |


### Response Header

**Contoh Response**

```shell
X-TIMESTAMP: 2025-06-22T14:29:07+07:00                                  
X-CLIENT-KEY: 22fd3727-3044-4596-8552-f4e54205f540 
{
    "responseCode": "2007400",
    "responseMessage": "Successful",
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAyNjEyOSIsImFjY291bnRJZCI6MjkxNTMsImFjY291bnRJZFBvaW50IjoxNDQyMiwibmJmIjoxNzUwNzM0NDMwLCJjYklkIjoiNjU0OGRjZjEtYzYwZS00NjlhLTg3MTktODYwZmY4NGEwMmM5IiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiY3JlZGVudGlhbElkIjowLCJleHAiOjE3NTIwMzA0MzAsImlhdCI6MTc1MDczNDQzMCwianRpIjoiYWRmMjM0ZjItN2E2OS00OTIzLTk5ZDYtZTExMTk3MDYwMDJmIn0.dYsDLsTr5WG2s_yYBb1rWMn7GpNktlMUYi5SrFoGRnFTQWoXys5XGXhSGoOdDxRpyQfy_CSv9cb-j3j0O38M8DV7AuBEGw9MuoHmf9goxIUA9bqaFFK5aY28m9wi-ra3AG3YPMR5rP85h25w9JdB2jTVJbywFNOZ2kp1W-kvAomzDUURedzD4CXkgiOU_v0F2i6XmzJmAQP9R2uHCDEmpTYsgdjAcs8-VCGAmEWNiySMp1MirHnWCAb6XbLqMqqczbDjzwPt3vQuYnFUaV7Iys2fEenQMFHiQVKMeY-j8l73wYEv-0TlJID43BS7JD9gn1cPGBDlVVGEtoD_PF8JDw",
    "tokenType": "Bearer",
    "accessTokenExpiryTime": "2025-07-09T10:07:10+07:00",
    "refreshToken": "174ddb0a-9116-4217-9fe6-1acd7c7c12fc",
    "refreshTokenExpiryTime": "2027-06-24T10:07:10+07:00"
}
```


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-CLIENT-KEY | String | Mandatory | Merchant/Partner Client ID |


### Response Body


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| accessToken | String | Mandatory | Authorization berupa string yang diberikan untuk pengguna dan digunakan untuk mengakses daya yang dilindungi |
| tokenType | String | Mandatory | Bearer Token |
| accessTokenExpiryTime | String | Mandatory | Waktu ketika accessToken akan expired. accessToken akan expired dalam waktu 15 hari dengan format ISO8601 |
| refreshToken | String | Mandatory | Random string yang digunakan pengguna untuk mendapatkan accessToken baru untuk mengakses data user. |
| refreshTokenExpiryTime | String | Mandatory | Waktu untuk refreshToken akan expired. |


### Response List


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4017400 | authCode Used | authCode sudah pernah dipakai |
| 4017400 | authCode expired | authCode sudah kedaluwarsa |
| 4017400 | refreshToken invalid | Refresh Token tidak sesuai |


### API Account Binding Inquiry

API ini digunakan untuk mengecek status binding customer dan bersifat opsional.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 08 |
| Channel ID | 01108 |
| Method | POST |
| URL Sandbox | /snap-service/snap/v1.0/registration-account-inquiry |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Account Binding Inquiry (01108) |


### Request Body

**Contoh cURL Account Binding Inquiry**

```shell
curl --location 'https://sandbox.astrapay.com/snap-service/snap/v1.0/registration-account-inquiry' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU95STd1R0g0dV9ZMzBmeTc4b1I1T2V6S3JIdVpzIn0.eyJleHAiOjE3MDY4Mzk3NTksImlhdCI6MTcwNjgwMzc1OSwianRpIjoiZmUyOTEyNzMtNTM1Ny00MmE1LWExODMtOWUxNWNiZTdjYjBmIiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay1zaXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwic3ViIjoiNDY2NTk5NzEtZTgwZC00OThiLTlkNjYtM2VhMTJjNDY5Mzk5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbImRlZmF1bHQtcm9sZXMtYXN0cmFwYXkiXX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudEhvc3QiOiIxODIuMjUzLjU5LjEwNSIsImNsaWVudElkIjoiYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwiY2xpZW50QWRkcmVzcyI6IjE4Mi4yNTMuNTkuMTA1In0.UhjrPznIeUiRMxptnrTeLN_Z6svcN0RzDsAU8oj_F965hRKVWDfp_B4kp2i1gnaC4Tp8X9jC-5xSEBicSZR4I-TitsuikPvT4l_NGqXcHCxSa4noeQmjoDysMILotTqa3rIx9_dWxvIyZil_t4b7L7fhTCVB-B6boxYpUGpBaNOxl6gdC8184AwFX-X8bqBTe2t9BaWZ6QBWAaarB4MdjYArrXW8N5C1oraPh4oAz2O2JVEhdM5ls1Jj0p2SlAVlsYSr9LbFJkwlJSJirDgjcArBbyFhx9OKLJT5F_7gvZm8iOBZuiUTOz4V0Sk79W-mwL0UcTIkCp1F-_PPfM0r3w' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2023-06-02T13:20:19+07:00' \
--header 'X-SIGNATURE: DMQt5kjGE40k+GayNs+tY1VYWoHfum6ASuiEUp51ituU0/yoA4ZhQGyMeRXjBkrbbYhRR+YB3aiIss7Y5b5p4w==' \
--header 'X-PARTNER-ID: 22fd3727-3044-4596-8552-f4e54205f540' \
--header 'X-EXTERNAL-ID: 9812839751201231223895' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'CHANNEL-ID: 01108' \
--data '{
    "additionalInfo": {
        "refreshToken": "57d21fe3-ba9c-4f2d-9fde-eae669bbf80d"
    }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| additionalInfo | Object | Mandatory | Informasi tambahan |
| additionalInfo.refreshToken | String | Mandatory | Informasi tambahan berupa refreshToken |


### Response Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |


### Response Body

**Contoh Response**

```shell
{
  "responseCode": "2000800",
  "responseMessage": "Successful",
  "referenceNo": "82c7ead2-ac97-49e0-a350-637712e46306",
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| referenceNo | String | Mandatory | ID transaksi pada AstraPay |


### API Account Unbinding

Layanan ini digunakan untuk memutuskan binding antara akun pengguna layanan dengan akun penyedia layanan. **Hubungi kami apabila ingin mengimplementasikan API ini.**

![img](./assets/snapAccountUnbinding-56e9395a_1.png)

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 09 |
| Channel ID | 01709 |
| Method | POST |
| URL Sandbox | snap-service/snap/v1.0/registration-account-unbinding |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk API Account Unbinding hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Account Unbinding (01709) |


### Request Body

**Contoh cURL Account Unbinding**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/snap-service/snap/v1.0/registration-account-unbinding' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE2NzI0MTA2MTQsImlhdCI6MTY3MjM3NDYxNCwianRpIjoiNTUyNWRiMTctZDIxNC00YmU4LWI1ZTQtYTAwNDUxODE1MjgzIiwiaXNzIjoiaHR0cDovLzEwLjIwLjcuNjo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjMzZTRhYTQ0LTU2M2QtNGE5NC05NjE2LWQ0MDdlZTZhZjc0NyIsInR5cCI6IkJlYXJlciIsImF6cCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMTAuNDAiLCJjbGllbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC1jMThlYzM1Zi01N2Q2LTRmNjktYjk3ZS1iMmVmMmNlNzhhYmIiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjEwLjQwIn0.c8LFOYl72UuvqGlc69kO7TuWLL_AkV1doAaZVzSwXA-6oyu85babY49T5DIElTTHGBWQXKBKOovnOmLBK9cJscshcm-dDm9kBGn8JI0yLjfHMa_-KLXFN8eedPu5pu936NIvO8rANGXo0b1pWoSKC4NgW8WxHZvzmUpN_H0-0WdTzFvbRRpcy2b1NrSa4xMwEa3tgdM8yIGFDDtK1l7X0KjSGlZ7LGarzbjf9yul9f6xNOcJOSMPu8zOWwYtiEKQabUm0wJSW2dvXYG_3VCeKeuhgEivOeoqzAEvq5a9AYVifOPwl-Hi_ba4DaH6wGohHcBjn9e6-xTTp7dWNxJCFQ' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2022-10-24T07:44:11+07:00' \
--header 'X-SIGNATURE: 7jkf3scp5kd8opCBnuWql+GayuKoyPD3vKsKGDyRsNbNEc/qzWgIvADGYmR8FSOU9FFmE4JJvR0JO9cYGqmlvg==' \
--header 'X-PARTNER-ID: c18ec35f-57d6-4f69-b97e-b2ef2ce78abb' \
--header 'X-EXTERNAL-ID: 008541234525416' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'CHANNEL-ID: 01709' \
--data-raw '{
  "merchantId": "9653109f-2806-48cf-ad75-85664c2db33e", 
  "additionalInfo":{              
      "refreshToken": "57d21fe3-ba9c-4f2d-9fde-eae669bbf80d"   
  }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| additionalInfo | Object | Mandatory | Informasi Tambahan |
| additionalInfo.refreshToken | String | Mandatory | Informasi tambahan berupa refreshToken |


### Response Body

**Contoh Response**

```shell
{
 "responseCode":"2000900", 
 "responseMessage":"Request has been processed successfully",
 "referenceNo":"2020102977770000000009", 
 "unlinkResult":"success"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| referenceNo | String | Mandatory | ID transaksi pada AstraPay |
| unlinkResult | String | Mandatory | Hasil dari proses unbinding |


### SNAP Direct Debit

Layanan ini digunakan untuk melakukan pembayaran di sisi customer dari rincian pembelian sampai mendapatkan notifikasi bahwa pembayaran sudah berhasil.

### Use Case Diagram

Berikut adalah use case diagram Direct Debit
![img](./assets/img_use_case_direct_debit-55720988_1.png)

Layanan ini terdiri dari 3 API, diantaranya:

 1. [API Direct Debit Payment](#api-direct-debit-payment)

 2. [API Direct Debit Payment Status](#api-direct-debit-payment-status)

 3. [API Direct Debit Payment Refund](#api-direct-debit-payment-refund)

 4. [API Direct Debit Payment Notify](#api-direct-debit-payment-notify)

## API Direct Debit Payment

API ini digunakan untuk melakukan pembayaran.

![img](./assets/directDebitPayment-c6ceebba_1.png)

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 54 |
| Channel ID | 00854 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/debit/payment-host-to-host |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Conditional | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c). Bersifat wajib ketika menggunakan metode pembayaran *payment with linking* |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk API Direct Debit hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Conditional | Identifier device yang digunakan oleh customer pada API yang sedang diakses. Bersifat wajib ketika menggunakan metode pembayaran payment with linking |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Direct Debit Payment (00854) |


### Request Body

**Contoh cURL Direct Debit Payment**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/debit/payment-host-to-host' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE2NzI0MTA2MTQsImlhdCI6MTY3MjM3NDYxNCwianRpIjoiNTUyNWRiMTctZDIxNC00YmU4LWI1ZTQtYTAwNDUxODE1MjgzIiwiaXNzIjoiaHR0cDovLzEwLjIwLjcuNjo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjMzZTRhYTQ0LTU2M2QtNGE5NC05NjE2LWQ0MDdlZTZhZjc0NyIsInR5cCI6IkJlYXJlciIsImF6cCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMTAuNDAiLCJjbGllbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC1jMThlYzM1Zi01N2Q2LTRmNjktYjk3ZS1iMmVmMmNlNzhhYmIiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjEwLjQwIn0.c8LFOYl72UuvqGlc69kO7TuWLL_AkV1doAaZVzSwXA-6oyu85babY49T5DIElTTHGBWQXKBKOovnOmLBK9cJscshcm-dDm9kBGn8JI0yLjfHMa_-KLXFN8eedPu5pu936NIvO8rANGXo0b1pWoSKC4NgW8WxHZvzmUpN_H0-0WdTzFvbRRpcy2b1NrSa4xMwEa3tgdM8yIGFDDtK1l7X0KjSGlZ7LGarzbjf9yul9f6xNOcJOSMPu8zOWwYtiEKQabUm0wJSW2dvXYG_3VCeKeuhgEivOeoqzAEvq5a9AYVifOPwl-Hi_ba4DaH6wGohHcBjn9e6-xTTp7dWNxJCFQ' \
--header 'Authorization-Customer: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJleHRlcm5hbFVpZCI6InVzZXJJZDEyMyIsInN1YiI6IjA4MjIzNTQwMTExMyIsIm5iZiI6MTY3MjMyMDc2NiwibWVyY2hhbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImlzcyI6IkFzdHJhUGF5LVNuYXAiLCJjbGFpbSI6IlNOQVAiLCJleHAiOjE2NzM2MTY3NjYsInR5cGUiOiJBQ0NFU1MiLCJpYXQiOjE2NzIzMjA3NjYsInVzZXJJZCI6MjAwMDI4LCJqdGkiOiIzZmNlMGYyZC01ZjQ2LTQxOTEtODFjYy05ZTE3MTE3MTU2MGQifQ.VDp_PXp_4xryNF74JUhlHnvcC-8agCLY2Ej5B5Sqh_t_pfZF-AqJZXQSr0dKFCDCnIzn_OsK1ydKoQmeQP4IJ3wV2Ep7QaG9VCRVR0WhgJMYB6BaiBpt4kZpryqAjPgHh7lYhSs4sTtxegxMT4IUK4Glw-yCZC_qgWEUFffh1VsT-JHNI0nIQbapVvPkOvuHBX2t_JqmGXz3BESH2-woaI3MEz-zbdPm7lJzN_A1QZAtVPgaZXdVMj9c9-3olcdoDyOj86X-EHW_O30GRKdxLm09ier31VRKj15U3E8Lgaw0IcYniqNNhLkp300fKf6_26eH0zeZrAIPwRxIHv5CSA ' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2022-10-23T07:44:11+07:00' \
--header 'X-SIGNATURE: 7jkf3scp5kd8opCBnuWql+GayuKoyPD3vKsKGDyRsNbNEc/qzWgIvADGYmR8FSOU9FFmE4JJvR0JO9cYGqmlvg==' \
--header 'X-PARTNER-ID: c18ec35f-57d6-4f69-b97e-b2ef2ce78abb' \
--header 'X-EXTERNAL-ID: 008541234525416' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'CHANNEL-ID: 00854' \
--data-raw '{
    "partnerReferenceNo": "TRX301222140301000",
    "merchantId":"ffedcd28-70ea-4540-9172-d335f5dda51c",
    "validUpTo":"2022-10-23T07:49:11+07:00",
    "amount": {
        "value": "10000.00",
        "currency": "IDR"
    },
    "additionalInfo": {
        "description": "Buy Starbucks"
    }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |
| merchantId | String | Conditional | Kode unik setiap Merchant yang diberikan AstraPay. Bersifat wajib ketika Merchant terdaftar memiliki Sub-Merchant pada sistem AstraPay |
| validUpTo | String | Optional | Waktu ketika pembayaran akan kedaluwarsa secara otomatis. Nilai maksimum: 15 menit dari waktu transaksi dibuat, nilai minimum: 3 menit, nilai default: 15 menit |
| amount | String | Mandatory | - |
| amount.value | String | Mandatory | Jumlah transaksi yang direquest oleh Merchant, termasuk 2 digit desimal. **Cth: 100000.00** |
| amount.currency | String(ISO4217) | Mandatory | Mata Uang |
| additionalInfo | Object | Mandatory | Informasi Tambahan |
| additionalInfo.description | String | Mandatory | Deskripsi |
| additionalInfo.paymentType | String | Conditional | Bersifat wajib ketika Merchant terdaftar menggunakan fitur **In-APP Payment** AstraPay (paymentType=IN_APP). **Hubungi kami apabila ingin mengimplementasikan fitur ini** |


### Response Body

**Contoh Response**

```shell
{
    "responseCode":"2005400",
    "responseMessage":"Successful",
    "partnerReferenceNo":"2020102900000000000001",
    "referenceNo":"INV/PAC/ONP/230223/26PIVDJFNW0",
    "webRedirectUrl": "https://sandbox.astrapay.com/merchant-service/payments/1c74a043-a28d-4826-a175-0bba189a83fd"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| partnerReferenceNo | String | Mandatory | ID transaksi pada Merchant/Partner |
| referenceNo | String | Mandatory | ID transaksi pada AstraPay |
| webRedirectUrl | String | Conditional | URL pembayaran AstraPay (Webview). **Cth: https://sandbox.astrapay.com/merchant-service/payments/1c74a043-a28d-4826-a175-0bba189a83fd** |
| appRedirectUrl | String | Conditional | URL pembayaran AstraPay (Apps). Response ini tersedia jika merchant menambahkan 'paymentType=IN_APP' pada 'additionalInfo'. **Cth: astrapay://payment-channel/payment?id={transactionId}** |


## API Direct Debit Payment Status

API ini digunakan untuk melakukan pengecekan status transaksi.

![img](./assets/directDebitPaymentStatus-ddb21b4f_1.png)

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 55 |
| Channel ID | 00155 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/debit/status |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format yyyy-MM-ddTHH:mm:ssTZD yang valuenya sama dengan yang digunakan pada signature-auth, token B2B Access Token Request, dan signature-service |
| X-SIGNATURE | String | Mandatory | Signature untuk API Direct Debit hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Direct Debit Payment Status (00155) |


### Request Body

**Contoh cURL Direct Debit Payment Status**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/debit/status' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICIzTF9udFF6MGZsNjl5UHB4T2RBTDk3NDNGOU05UWszVklTOWMwZGNwa2VFIn0' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2020-01-01T00:00:00+07:00' \
--header 'X-SIGNATURE: v3yjqcO1MpqNINgjkGiqkSeeGypreVjDHZQKe8qxu0rr5vGzFmQQeOIviryOqQgQ/nEdeWN5eu47xedeEG3a0Q==' \
--header 'X-PARTNER-ID: c18ec35f-57d6-4f69-b97e-b2ef2ce78abb' \
--header 'X-EXTERNAL-ID: 12345678s' \
--header 'CHANNEL-ID: 00155' \
--data-raw '{
   "originalPartnerReferenceNo":"TRANSACTION-TEST-05",
   "originalReferenceNo":"",
   "serviceCode":"54",
   "amount":{
      "value":"100000.00",
      "currency":"IDR"
   }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi dari Merchant/Partner (Merchant/Partner Transaction ID) |
| originalReferenceNo | String | Optional | ID transaksi dari AstraPay |
| serviceCode | String | Mandatory | Kode service yang diakses |
| amount | Object | Mandatory | - |
| amount.value | String | Mandatory | Jumlah transaksi yang direquest oleh Merchant, termasuk 2 digit desimal. **Cth: 100000.00** |
| amount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |


### Response Body

**Contoh Response**

```shell
{
   "responseCode":"2005500",
   "responseMessage":"Request has been processed successfully",
   "originalPartnerReferenceNo":"2020102900000000000001", //merchantTransactionId
   "originalReferenceNo":"2020102977770000000009",
   "serviceCode":"54",
   "latestTransactionStatus":"00",
   "refundHistory":[      
         {
         "refundNo":"96194816941239812",
         "partnerReferenceNo":"239850918204981205970",
         "refundAmount":{
            "value":"100000.00",
            "currency":"IDR"
         },
         "refundStatus":"00",
         "refundDate":"2020-12-23T07:44:16+07:00",
         "reason":"Sayur 1 hilang"
      },
      {
         "refundNo":"96194123981251341",
         "partnerReferenceNo":"2398509123131981205970",
         "refundAmount":{
            "value":"100000.00",
            "currency":"IDR"
         },
         "refundStatus":"00",
         "refundDate":"2020-12-23T07:54:16+07:00",
         "reason":"Sayur 2 rusak"
      }
   ],
   "transAmount":{
      "value":"100000.00",
      "currency":"IDR"
   },
   "feeAmount":{
      "value":"2000.00",
      "currency":"IDR"
   },
   "paidTime":"2020-12-21T14:56:11+07:00",
   "additionalInfo":{
      "amount": {
        "value": "10000.00",
        "currency": "IDR"
      },
      "payMethod": "Balance",
      "payOption": "AstraPay"
  }
   }
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi dari Merchant/Partner (Merchant/Partner Transaction ID) |
| originalReferenceNo | String | Mandatory | ID transaksi dari AstraPay |
| serviceCode | String | Mandatory | Kode service yang diakses |
| latestTransactionStatus | String | Mandatory | 00 - Success 01 - Initiated 02 - Paying 03 - Pending 04 - Refunded 05 - Canceled 06 - Failed 07 - Not found |
| refundHistory | Object | Optional | Riwayat refund, jika belum pernah refund akan kosong |
| refundNo | String | Conditional | ID refund |
| partnerReferenceNo | String | Mandatory | ID refund dari Merchant/Partner |
| refundAmount | Object | Mandatory | - |
| refundAmount.value | String | Mandatory | Jumlah saldo yang dikembalikan kepada Customer, termasuk 2 digit desimal. **Cth: 100000.00** |
| refundAmount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| refundStatus | String | Mandatory | Status refund |
| refundDate | String | Conditional | Tanggal refund |
| reason | String | Optional | Alasan refund |
| transAmount | Object | Mandatory | - |
| transAmount.value | String | Mandatory | Jumlah final transaksi yang dibayarkan oleh Customer, termasuk 2 digit desimal. **Cth: 100000.00** |
| transAmount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| feeAmount | Object | Mandatory | - |
| feeAmount.value | String | Mandatory | Jumlah biaya layanan dan biaya penanganan yang dibayarkan oleh customer, termasuk 2 digit desimal. **Cth: 2000.00** |
| feeAmount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| paidTime | String | Conditional | Tanggal transaksi |
| additionalInfo | Object | Conditional | Informasi tambahan transaksi. Akan diberikan apabila Merchant memiliki 2 payOption aktif (AstraPay dan Bank Saqu). Secara default Merchant hanya memiliki payOption **AstraPay**, ntuk mengaktifkan payOption **Bank Saqu**, silakan menghubungi tim AstraPay |
| additionalInfo.amount | Object | Conditional | - |
| additionalInfo.amount.value | String | Mandatory | Jumlah transaksi yang direquest oleh Merchant, termasuk 2 digit desimal. **Cth: 100000.00** |
| additionalInfo.amount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| additionalInfo.payMethod | String | Mandatory | Metode pembayaran yang digunakan oleh Customer (Saat ini AstraPay hanya memiliki metode pembayaran 'Balance') |
| additionalInfo.payOption | String | Mandatory | Opsi pembayaran yang digunakan oleh Customer (Saat ini AstraPay hanya memiliki opsi pembayaran 'AstraPay' dan 'Bank Saqu') |


## API Direct Debit Payment Notify

API ini digunakan untuk melakukan  callback dari AstraPay ke merchant.

![img](./assets/directDebitPaymentNotify-50198468_1.png)

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 56 |
| Channel ID | 00656 |
| Method | POST |
| URL Sandbox | /v1.0/debit/notify |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** yang valuenya sama dengan yang digunakan pada signature-auth, token B2B Access Token Request, dan signature-service |
| X-SIGNATURE | String | Mandatory | Signature untuk API Direct Debit hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Payment Notify (00656) |


### Request Body

**Contoh cURL Direct Debit Payment Notify**

```shell
curl --location --request POST 'https://merchant.com/v2/snap/v1.0/debit/notify' \
--header 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJhcHByZXN0c2VydmljZSIsImF1ZCI6InNsYXNocm9vdCIsImlhdCI6MTY3MTEzNDUyMSwibmJmIjoxNjcxMTM0NTIxLCJleHAiOjE2NzExMzgxMjEsImRhdGEiOnsidWlkIjoiZDBmNTU0NTJjM2E2MmQ0MDNlNjNiNDRmZGFiMGY4NGU0NTZhYjc0YTIwMjIxMjE2MDMwMjAxIiwidWlwIjoiYjNmNjM0NjY5OTQwZTA4YWFlZDQ1NDZhMGZlMjFmNjJkMWJjNDcwMDIwMjIxMjE2MDMwMjAxIn19.jGoDTMu0H7-nxdmQnTOnRiJIucxjvB1yokOsuoIzWbA' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2022-12-15T17:07:16+07:00' \
--header 'X-SIGNATURE: EwSP45pjvkOTJ59OJkjbhKQwqQuonulSZ0yiswz6q3r0U0OgWest15xOAdQ8Fu5lY6+wRJIt3a3J9i6SHO6OUw==' \
--header 'X-PARTNER-ID: dcb60ae4-4d38-11ec-81d3-0242ac130003' \
--header 'X-EXTERNAL-ID: 167284702245712690811679223447346553' \
--header 'CHANNEL-ID: 00656' \
--data-raw '{
   "originalPartnerReferenceNo":"AP2210110252200023",
   "originalReferenceNo":"INV/PAY/ONP/221215/NP5BYX56D26",
   "merchantId":"1478c1f7-f06d-426b-95cd-034cd085910e",
   "amount":{
      "value":"10000.00",
      "currency":"IDR"
   },
   "latestTransactionStatus":"00",
   "finishedTime":"2022-12-15T17:07:15+07:00",
   "additionalInfo": {
      "transAmount": {
        "value": "10000.00",
        "currency": "IDR"
      },
      "feeAmount": {
        "value": "2000.00",
        "currency": "IDR"
      },
      "payMethod": "Balance",
      "payOption": "AstraPay"
  }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi dari Merchant/Partner (Merchant Transaction ID) |
| originalReferenceNo | String | Mandatory | ID transaksi dari AstraPay |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| amount | String | Mandatory | - |
| amount.value | String | Mandatory | Jumlah transaksi yang direquest oleh Merchant, termasuk 2 digit desimal. **Cth: 100000.00** |
| amount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| latestTransactionStatus | String | Mandatory | 00 - Success 01 - Initiated 02 - Paying 03 - Pending 04 - Refunded 05 - Canceled 06 - Failed 07 - Not found |
| finishedTime | String | Mandatory | Waktu transaksi selesai |
| additionalInfo | Object | Conditional | Informasi tambahan transaksi. Akan diberikan apabila Merchant memiliki 2 payOption aktif (AstraPay dan Bank Saqu). Secara default Merchant hanya memiliki payOption **AstraPay**, ntuk mengaktifkan payOption **Bank Saqu**, silakan menghubungi tim AstraPay |
| additionalInfo.transAmount | Object | Conditional | - |
| additionalInfo.transAmount.value | String | Mandatory | Jumlah final transaksi yang dibayarkan oleh customer, termasuk 2 digit desimal. **Cth: 10000.00** |
| additionalInfo.transAmount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| additionalInfo.feeAmount | Object | Conditional | - |
| additionalInfo.feeAmount.value | String | Mandatory | Jumlah biaya layanan dan biaya penanganan yang dibayarkan oleh customer, termasuk 2 digit desimal. **Cth: 2000.00** |
| additionalInfo.feeAmount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| additionalInfo.payMethod | String | Mandatory | Metode pembayaran yang digunakan oleh Customer (Saat ini AstraPay hanya memiliki metode pembayaran 'Balance') |
| additionalInfo.payOption | String | Mandatory | Opsi pembayaran yang digunakan oleh Customer (Saat ini AstraPay hanya memiliki opsi pembayaran 'AstraPay' dan 'Bank Saqu') |


### Response Body

**Contoh Response**

```shell
{
   "responseCode":"2005600",
   "responseMessage":"Request has been processed successfully"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |




## API Direct Debit Payment Refund

API ini digunakan untuk melakukan pengembalian saldo ke user atas transaksi *direct debit*. **Hubungi kami apabila ingin mengimplementasikan API ini**

![img](./assets/directDebitPaymentRefund-e5295bec_1.png)

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 58 |
| Channel ID | 00258 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/debit/refund |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk API Direct Debit Payment Refund hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Conditional | Identifier device yang digunakan oleh customer pada API yang sedang diakses. Bersifat wajib ketika menggunakan metode pembayaran payment with linking |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Direct Debit Payment Refund (00258) |


### Request Body

**Contoh cURL Direct Debit Payment Refund**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/debit/refund' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU95STd1R0g0dV9ZMzBmeTc4b1I1T2V6S3JIdVpzIn0.eyJleHAiOjE3MTM3OTcyNzcsImlhdCI6MTcxMzc2MTI3NywianRpIjoiNmI1NjNkMTUtZDExZC00MGYwLWI0MGMtYzE3MmQ4YzUyNWVkIiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay1zaXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwic3ViIjoiNDY2NTk5NzEtZTgwZC00OThiLTlkNjYtM2VhMTJjNDY5Mzk5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbImRlZmF1bHQtcm9sZXMtYXN0cmFwYXkiXX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudEhvc3QiOiIzNC4xMDEuMjEwLjE5NiIsImNsaWVudElkIjoiYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtYzMzYWU3MTYtMjQwMi00OTc5LWIyMmQtMThhZmEyOGQzZDRlIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTAuMTk2In0.cF8G8YEIioE8CvxVBjNRtZRJToLXj4MCPHZEamkpmopGup_KhkDueWpIE_pRbaBtMz_WtO5ouxfJg21-SBahlAaDl49J5bbBKXObEjjpi7lbUkrn4RWQPPoUJLrm95vDyOLFBr5RoYVkc_RS4rVcW64GJskmNokygjFdMo5qymkT2UCyXZl0zPMkN6aTsaaDeicBFXtxyiIblJSMJQfxiLH5bE07PMGnxMdMihM79QoeotrdnYBoHdsbv0uYkR18Iss08fmpM7XzuMDilgccmZRR3zYI6A2V2TLXwtrnvB1bYTggEw1E5HNKF9ynljDmc_ZpJoFH7FuWo1kb2DdvgQ' \
--header 'Content-Type: application/json' \
--header 'X-SIGNATURE: +ECf18Ykl7aEnZcvfyZla8XjB6iRx++UP1jTALFRUckRUYXK1ZTh+VngVoZXCja5W6kNDgE/MCKueLWGF5C6ww==' \
--header 'X-TIMESTAMP: 2024-04-10T13:20:19+07:00' \
--header 'X-PARTNER-ID: c33ae716-2402-4979-b22d-18afa28d3d4e' \
--header 'X-EXTERNAL-ID: 1290831208480120239' \
--header 'CHANNEL-ID: 00258' \
--data-raw '{
   "originalPartnerReferenceNo":"TRX3012221403010037",
   "partnerRefundNo":"RFNDTST123",
   "refundAmount":{
      "value":"10000.00",
      "currency":"IDR"
   },
   "reason":"Customer complain"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| OriginalPartnerReferenceNo | String | Mandatory | ID transaksi dari Merchant/Partner (Merchant Transaction ID) |
| partnerRefundNo | String | Mandatory | ID refund dari Merchant/Partner |
| refundAmount | Object | Mandatory | - |
| refundAmount.value | String | Mandatory | Jumlah saldo yang dikembalikan kepada Customer, termasuk 2 digit desimal. **Cth: 10000.00**.  Ketentuan refund berdasarkan `payOption`: • **AstraPay**: Mendukung full refund (`refundAmount.value = transAmount`) dan partial refund (`refundAmount.value < transAmount`). Total refund kumulatif tidak boleh melebihi `transAmount`. • **Bank Saqu (BSQ)**: Hanya mendukung full refund. Nilai `refundAmount.value` harus sama dengan `transAmount`. Partial refund tidak dapat dilakukan |
| refundAmount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| reason | String | Optional | Alasan pengembalian saldo |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2005800",
    "responseMessage": "Successful",
    "originalPartnerReferenceNo": "TRX3012221403010037",
    "originalReferenceNo": "INV/PAC/ONP/240405/107RDGLOFW0",
    "refundNo": "INV/REF/ONP/240422/5056259LP1B",
    "partnerRefundNo": "RFNDTST123",
    "refundAmount": {
        "value": "10000.00",
        "currency": "IDR"
    },
    "refundTime": "2024-04-22T11:50:56.837811",
    "additionalInfo": {
        "amount": {                
          "value": "10000.00",
          "currency": "IDR"
        },
        "payMethod": "Balance",
        "payOption": "AstraPay"
  }
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi dari Merchant/Partner (Merchant Transaction ID) |
| OriginalReferenceNo | String | Mandatory | ID transaksi dari AstraPay |
| refundNo | String | Mandatory | ID refund dari AstraPay |
| partnerRefundNo | String | Mandatory | ID refund dari Merchant/Partner |
| refundAmount | Object | Mandatory | - |
| refundAmount.value | String | Mandatory | Jumlah saldo yang dikembalikan kepada Customer, termasuk 2 digit desimal.  **Cth: 10000.00** |
| refundAmount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| refundTime | String | Mandatory | Waktu pengembalian saldo |
| additionalInfo | Object | Conditional | Informasi tambahan transaksi. Akan diberikan apabila Merchant memiliki 2 payOption aktif (AstraPay dan Bank Saqu). Secara default Merchant hanya memiliki payOption **AstraPay**, ntuk mengaktifkan payOption **Bank Saqu**, silakan menghubungi tim AstraPay |
| additionalInfo.amount | Object | Conditional | - |
| additionalInfo.amount.value | String | Mandatory | Jumlah transaksi yang direquest oleh merchant, termasuk 2 digit desimal. **Cth: 10000.00** |
| additionalInfo.amount.currency | String(ISO4217) | Mandatory | Kode mata uang berdasarkan ISO (IDR) |
| additionalInfo.payMethod | String | Mandatory | Metode pembayaran yang digunakan oleh Customer (Saat ini AstraPay hanya memiliki metode pembayaran 'Balance') |
| additionalInfo.payOption | String | Mandatory | Opsi pembayaran yang digunakan oleh Customer (Saat ini AstraPay hanya memiliki opsi pembayaran 'AstraPay' dan 'Bank Saqu') |


## API Top Up Instruction (BETA)

API ini digunakan untuk menampilkan instruksi top up ke AstraPay yang dapat dilakukan oleh user melalui berbagai channel seperti transfer bank, e-wallet, dan lainnya. **Hubungi kami apabila ingin mengimplementasikan API ini**

![img](./assets/topUpWebview-32648cab_1.png)

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Method | POST |
| URL Sandbox | /merchant-service/v1.0/top-up-webview |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c). Bersifat wajib ketika menggunakan metode pembayaran *top Up Instruction* |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk API Direct Debit hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Conditional | Identifier device yang digunakan oleh customer pada API yang sedang diakses. Bersifat wajib ketika menggunakan metode pembayaran payment with linking |


**Contoh cURL Top Up Instruction**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/v1.0/top-up-webview' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE2NzA5MzgzNjAsImlhdCI6MTY3MDkwMjM2MCwianRpIjoiYTZmMWIzNWItM2Y0NS00ZjJmLThiYzQtODQ0ZDcxOTE1NzNkIiwiaXNzIjoiaHR0cDovLzEwLjIwLjcuNjo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjMzZTRhYTQ0LTU2M2QtNGE5NC05NjE2LWQ0MDdlZTZhZjc0NyIsInR5cCI6IkJlYXJlciIsImF6cCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMTAuMzkiLCJjbGllbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC1jMThlYzM1Zi01N2Q2LTRmNjktYjk3ZS1iMmVmMmNlNzhhYmIiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjEwLjM5In0.XF8epufqemFGyb94GKz53O4lXQ_jyF2UBuyfc4DagWDgGWBKQVosg_VrEwAVdjgeakQQHIDc7Bq3kCfBt1AGxJjASxpsdgpAhx9DmhsfK8YQnK2-WPyvjHIdAA1Ws8Amr8ccCs5iSlXd5Vz6gjnu5ETfxFOEJztu-RtURM7lwnkp8P4rHTf47lZEpC6zAGqasKdUE1AtVP3GW6arMCB0SE-2roWoP1bFMYq802fHcIqdIT0Egc55UnQbsXYfUbrDZ1YvuVJehUamn1PJe5HWZ2PyYGMqhn4efE5xElfWLyeJO2llbNPrCLBgRrgGqYwzWPCuZRKpCnJNyHne3_3oZA' \
--header 'Authorization-Customer: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJleHRlcm5hbFVpZCI6InVzZXJJZDEyMyIsInN1YiI6IjA4MjIzNTQwMTExMyIsIm5iZiI6MTY3MDkwMjQ4MiwibWVyY2hhbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImlzcyI6IkFzdHJhUGF5LVNuYXAiLCJjbGFpbSI6IlNOQVAiLCJleHAiOjE2NzIxOTg0ODIsInR5cGUiOiJBQ0NFU1MiLCJpYXQiOjE2NzA5MDI0ODIsInVzZXJJZCI6MjAwMDI4LCJqdGkiOiJkMDRhYWYzOS03YjEyLTQ1YzAtODYzYS1lYTIzMDhkNzJiMTUifQ.WdAwnIzcN7aobItqxdWjbb7FdodlK73_qs8HVBOZu5gCjMK5PyuP2vWL49hfuiDm85IlTOs5S9HnEdqHGPOhfdORa6pnkxX9v884N-Nq-eRoBbYSHsZTzvNGKlg_zvFBHveC6_GCxCwIzKzXMtIJoi2_ZmrgupRbW9KZTqzqUWIYYEjCMQS-dGh3s79iQ2cIiUD-PonINnXTtaq31fBPd2TT0nHoXrYzxVvN2Vzq289ivzb0A3tx2tmFdCkbzKrYstBhcQ3vragu7-TiJ2kR9Mf_o7K6jVulOpCr-tmxawblJPDnWJqzyVJtgnRB7j0W9n4pw3USohWz_Yd_t5GebQ' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2022-10-23T07:44:11+07:00' \
--header 'X-SIGNATURE: xxxTuetiAq4Iv0gTmP+9R6jF1glQf85sS48qRrZHZHGrEEeDE+RummnSCVZiZuJTkaLOywNkcQxx' \
--header 'X-PARTNER-ID: c18ec35f-57d6-4f69-b97e-b2ef2ce78abb' \
--header 'X-EXTERNAL-ID: 003111234567890124s2s2r' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--data-raw '{}'
```

### Response Body

**Contoh Response**

```shell
{
    "webRedirectUrl": "https://sandbox.astrapay.com/payment-channel/top-up?sessionId=11212a89-a688-42f0-93a6-5b514f60b4e1"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| webRedirectUrl | String | Mandatory | URL Top Up AstraPay (Webview). |


## SNAP Balance Inquiry

Layanan ini digunakan untuk mengecek saldo customer yang hanya terdiri dari 1 API, yaitu [API Balance Inquiry](#api-balance-inquiry).

### Use Case Diagram

Berikut adalah use case diagram Balance Inquiry
![img](./assets/img_use_case_balance_inquiry-c6961c69_1.png)

Berikut ini adalah flow Balance Inquiry.
![img](./assets/umlBalanceInquiryPaymentChannel-60362120_1.png)

### API Balance Inquiry

API ini digunakan untuk mengetahui jumlah saldo customer.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 11 |
| Channel ID | 00311 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/balance-inquiry |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Authorization | String | Mandatory | Bearer token dari hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c). Bersifat wajib ketika menggunakan metode pembayaran *payment with linking* |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Signature untuk API Direct Debit hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Conditional | Identifier device yang digunakan oleh customer pada API yang sedang diakses. Bersifat wajib ketika menggunakan metode pembayaran payment with linking |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Balance Inquiry (00311) |


### Request Body

**Contoh cURL Balance Inquiry**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/balance-inquiry' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE2NzA5MzgzNjAsImlhdCI6MTY3MDkwMjM2MCwianRpIjoiYTZmMWIzNWItM2Y0NS00ZjJmLThiYzQtODQ0ZDcxOTE1NzNkIiwiaXNzIjoiaHR0cDovLzEwLjIwLjcuNjo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjMzZTRhYTQ0LTU2M2QtNGE5NC05NjE2LWQ0MDdlZTZhZjc0NyIsInR5cCI6IkJlYXJlciIsImF6cCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMTAuMzkiLCJjbGllbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC1jMThlYzM1Zi01N2Q2LTRmNjktYjk3ZS1iMmVmMmNlNzhhYmIiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjEwLjM5In0.XF8epufqemFGyb94GKz53O4lXQ_jyF2UBuyfc4DagWDgGWBKQVosg_VrEwAVdjgeakQQHIDc7Bq3kCfBt1AGxJjASxpsdgpAhx9DmhsfK8YQnK2-WPyvjHIdAA1Ws8Amr8ccCs5iSlXd5Vz6gjnu5ETfxFOEJztu-RtURM7lwnkp8P4rHTf47lZEpC6zAGqasKdUE1AtVP3GW6arMCB0SE-2roWoP1bFMYq802fHcIqdIT0Egc55UnQbsXYfUbrDZ1YvuVJehUamn1PJe5HWZ2PyYGMqhn4efE5xElfWLyeJO2llbNPrCLBgRrgGqYwzWPCuZRKpCnJNyHne3_3oZA' \
--header 'Authorization-Customer: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJleHRlcm5hbFVpZCI6InVzZXJJZDEyMyIsInN1YiI6IjA4MjIzNTQwMTExMyIsIm5iZiI6MTY3MDkwMjQ4MiwibWVyY2hhbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImlzcyI6IkFzdHJhUGF5LVNuYXAiLCJjbGFpbSI6IlNOQVAiLCJleHAiOjE2NzIxOTg0ODIsInR5cGUiOiJBQ0NFU1MiLCJpYXQiOjE2NzA5MDI0ODIsInVzZXJJZCI6MjAwMDI4LCJqdGkiOiJkMDRhYWYzOS03YjEyLTQ1YzAtODYzYS1lYTIzMDhkNzJiMTUifQ.WdAwnIzcN7aobItqxdWjbb7FdodlK73_qs8HVBOZu5gCjMK5PyuP2vWL49hfuiDm85IlTOs5S9HnEdqHGPOhfdORa6pnkxX9v884N-Nq-eRoBbYSHsZTzvNGKlg_zvFBHveC6_GCxCwIzKzXMtIJoi2_ZmrgupRbW9KZTqzqUWIYYEjCMQS-dGh3s79iQ2cIiUD-PonINnXTtaq31fBPd2TT0nHoXrYzxVvN2Vzq289ivzb0A3tx2tmFdCkbzKrYstBhcQ3vragu7-TiJ2kR9Mf_o7K6jVulOpCr-tmxawblJPDnWJqzyVJtgnRB7j0W9n4pw3USohWz_Yd_t5GebQ' \
--header 'Content-Type: application/json' \
--header 'X-TIMESTAMP: 2022-10-23T07:44:11+07:00' \
--header 'X-SIGNATURE: xxxTuetiAq4Iv0gTmP+9R6jF1glQf85sS48qRrZHZHGrEEeDE+RummnSCVZiZuJTkaLOywNkcQxx' \
--header 'X-PARTNER-ID: c18ec35f-57d6-4f69-b97e-b2ef2ce78abb' \
--header 'X-EXTERNAL-ID: 003111234567890124s2s2r' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'CHANNEL-ID: 00311' \
--data-raw '{
    "balanceTypes": ["BALANCE", "POINT"]
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| balanceTypes | Array of String | Mandatory | Menentukan jenis saldo customer yang ingin ditampilkan (Saat ini AstraPay hanya memiliki jenis saldo "BALANCE" yang bersifat mandatory dan jenis saldo "POINT" yang bersifat optional) |


### Response Body

**Contoh Response**

```shell
{
   "responseCode":"2001100",
   "responseMessage":"Successful",
   "accountInfos":[
      {
         "balanceType":"BALANCE",
         "amount":{
            "value":"10000.00",
            "currency":"IDR"
         }
      },
      {
         "balanceType":"POINT",
         "amount":{
            "value":"100.00",
            "currency":"IDR"
         }
      }
   ],
   "additionalInfo":[
         "userStatus" : "LOCKED"
   ]
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [Lihat response list](#response-list-payment-channel) |
| accountInfos | List | Optional | Berisikan informasi mengenai saldo customer |
| balanceType | String | Mandatory | Jenis saldo |
| amount | Object | Optional | - |
| amount.value | String | Mandatory | Jumlah bersih transaksi yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| amount.currency | String(ISO4217) | Mandatory | Mata uang |
| additionalInfo | Object | Optional | Informasi tambahan |
| additionalInfo.userStatus | String | Mandatory | Status akun user (ACTIVE/LOCKED/INCLOSURE) |


## SNAP Auth Payment

Layanan ini digunakan untuk dapat melakukan pembayaran (*Pre-Authorization*). Saat ini, layanan auth payment AstraPay hanya mendukung transaksi tanpa PIN. Agar dapat menggunakan layanan ini, merchant perlu menghubungkan akun *customer* terlebih dahulu menggunakan layanan berikut [**SNAP Registration**](#snap-registration).

### Sequence Diagram

Berikut adalah *sequence diagram* untuk proses *Auth Payment*:
![img](./assets/snapAuthPayment-ce2a60cd_1.png)

Layanan ini terdiri dari 7 API, diantaranya:


| Nama API | Deskripsi |
| --- | --- |
| [**API Auth Payment**](#api-auth-payment) | Digunakan untuk melakukan *reserve balance customer*. Hanya dapat digunakan apabila akun *customer* sudah terhubung |
| [**API Payment Query**](#api-payment-query) | Digunakan untuk mengecek status *reserve customer*. *Mandatory* digunakan apabila merchant mendapatkan response "internal server error" ketika hit API Auth Payment |
| [**API Capture**](#api-capture) | Digunakan untuk memotong *balance customer* yang sebelumnya sudah berhasil ter-*reserve* |
| [**API Capture Query**](#api-capture-query) | Digunakan untuk mengecek status *capture customer*. *Mandatory* digunakan apabila merchant mendapatkan response "internal server error" ketika hit API Capture |
| [**API Void**](#api-void) | Digunakan untuk membatalkan transaksi yang sudah berhasil ter-*reserve* namun belum ter-*capture* |
| [**API Void Query**](#api-void-query) | Digunakan untuk mengecek status *void customer*. *Mandatory* digunakan apabila merchant mendapatkan response "internal server error" ketika hit API Void |
| [**API Auth Refund**](#api-auth-refund) | Digunakan untuk melakukan pengembalian saldo kepada *customer* atas transaksi Auth Payment |


### API Auth Payment

API ini digunakan untuk melakukan *reserve balance customer* tanpa benar-benar mengurangi saldo customer sejak awal API ini dipanggil.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 63 |
| Channel ID | 01963 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/auth/payment |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Auth Payment (01963) |


### Request Body

**Contoh cURL Auth Payment**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/auth/payment' \
--header 'X-TIMESTAMP: 2024-12-01T23:12:51+07:00' \
--header 'X-SIGNATURE: qgmpONHPt/5u54tRYf8w3nuBzvqX7wjAj5dfyWqgPrGABFVHvETxW22kNqiLDNGr8zI3H+qpXNiQHo1yMLLeiw==' \
--header 'X-EXTERNAL-ID: 47941397849362225487741579324988' \
--header 'X-PARTNER-ID: 22fd3727-3044-4596-8552-f4e54205f599' \
--header 'CHANNEL-ID: 01963' \
--header 'Authorization-Customer: Bearer eyJ0eYAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAwMTYwNCIsImFjY291bnRJZCI6MTUyMTYsImFjY291bnRJZFBvaW50IjoxMzYzMiwibmJmIjoxNzMzMDY5NTU0LCJjYklkIjoiZWExMWM2MTgtMDc0MC00ZDQyLTg1YWQtNjNjZWQxMjIwMzFmIiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiZXhwIjoxNzM0MzY1NTU0LCJpYXQiOjE3MzMwNjk1NTQsImp0aSI6ImFjY2I0YTBmLTg3NmEtNDVlZi1iMDFjLTkwMmNiZDZmNTFhYiJ9.Q7tjaQxcprfmA_bbCDhkiHW7iEwnVDFL_iyodIO_iWgSyE_pqHXpYI0ls-lRt-QtTzP53FlFh39_dutipNWYQL9sl4yLwFhdrdXaO04gchYJD-KyorzQcUoV8O6Wvd0QGxSwSYlvcNIX9UvRcJmDEIfGTa95b4_nIGSLOiYrt5RK2SghNz1SxHgpWa5jZKdMdkqmGQIrEkXKPSpH051snHGQPwdgZcBQyhkacPKAs9UtqqoMdgBuffufFBbSpJPPCTcboi7A3PYetQ7vh7n9X-Xs04BXgkNj4qMWbmdHZFK1Bu5okulBznIpRakN_AOyMFOR0t_ACjogbuDJndfnsg' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3MzMxMDU1MDgsImlhdCI6MTczMzA2OTUwOCwianRpIjoiMjlkZTVlMTMtZTY5Ny00ZjAwLWI3ZTAtZmJhNDQ2ODE0OTU0IiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjkwMWRkNWMzLTJmZTAtNDJmMC1iZTYwLWNhYmI2OGE1MWZlMCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjIyZmQzNzI3LTMwNDQtNDU5Ni04NTUyLWY0ZTU0MjA1ZjU0MCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudElkIjoiMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50SG9zdCI6IjM0LjEwMS4yMTYuMTY4IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTYuMTY4In0.CLgxwgwAZTwt4siqYiwc5nLCW52gqtGi_c-FFpmPZsZzdqjWGfFYIeB7aOSzt5-rjWrplQJ3Xb_cXmznqNcIjpiGQ7_Qp7v1TfMhp5tkn-bR-LsOQ4bnF1muyioy6lRHqWytSDEq7orxPlsBTU4Sumwrzv_-DvLBRqQD565ZWtsPqTaByIfQcHDqqKM_pSn80T9vo5Lt5sebJ6ttM4Z7Xyir5rhO93yZLAXiz-Ih8xoOQgZawfvICxhO4UM2LgPtzijqWGo9W2XpD3M7rXEFXedxqKzJwFwXpQhWeBv__RWNDIAQ6fAEucx4QDjbZtdBAxb6u-TQrXIsCQoNwWJm8A' \
--header 'Content-Type: application/json' \
--data-raw '{
   "partnerReferenceNo":"1231231123",
   "merchantId":"9653109f-2806-48cf-ad75-85664c2db33e",
   "amount":{
      "value":"2000.00",
      "currency":"IDR"
   },
   "title":"Ikan bakar bumbu kuning",
   "additionalInfo":{
      "validUpTo":"2024-12-29T17:02:56" 
   }
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| partnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| amount | Object | Mandatory | - |
| amount.value | String | Mandatory | Jumlah transaksi *reserve* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| amount.currency | String(ISO4217) | Mandatory | Mata Uang |
| title | String | Mandatory | Judul order |
| additionalInfo | Object | Optional | Informasi tambahan |
| additionalInfo.validUpTo | String | Optional | Waktu ketika pembayaran akan kedaluwarsa secara otomatis.Nilai maksimum: 90 hari dari waktu transaksi, nilai minimum: 1 menit, nilai default: 1 hari |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2006300",
    "responseMessage": "Successful",
    "referenceNo": "INV/PAC/ONP/241201/70URMSYMQX0",
    "partnerReferenceNo": "1231231123",
    "amount": {
        "value": "2000.00",
        "currency": "IDR"
    },
    "paidTime": "2024-12-01T23:12:52.929051"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| referenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| partnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| amount | Object | Mandatory | - |
| amount.value | String | Mandatory | Jumlah bersih transaksi yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| amount.currency | String(ISO4217) | Mandatory | Mata Uang |
| paidTime | String | Mandatory | Tanggal transaksi *reserve* dibayarkan |


### API Payment Query

API ini digunakan untuk mengecek status *reserve customer*. Mandatory digunakan apabila merchant mendapatkan response "internal server error" ketika hit API Auth Payment.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 64 |
| Channel ID | 02064 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/auth/query |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Payment Query (02064) |


### Request Body

**Contoh cURL Payment Query**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/auth/query' \
--header 'X-TIMESTAMP: 2024-12-01T23:17:04+07:00' \
--header 'X-SIGNATURE: 6RQdrzmtX0DkdyM0WEQX2MF8ellBnU2fuM+MAkb+881tYiutxUq2fj4tmJmioxeqDg2iXx0HrhTZg8MdX+oJ7w==' \
--header 'X-EXTERNAL-ID: 249564970507005404065489' \
--header 'X-PARTNER-ID: 22fd3227-3044-4596-8552-f4e54205f540' \
--header 'CHANNEL-ID: 02064' \
--header 'Authorization-Customer: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAwMTYwNCIsImFjY291bnRJZCI6MTUyMTYsImFjY291bnRJZFBvaW50IjoxMzYzMiwibmJmIjoxNzMzMDY5NTU0LCJjYklkIjoiZWExMWM2MTgtMDc0MC00ZDQyLTg1YWQtNjNjZWQxMjIwMzFmIiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiZXhwIjoxNzM0MzY1NTU0LCJpYXQiOjE3MzMwNjk1NTQsImp0aSI6ImFjY2I0YTBmLTg3NmEtNDVlZi1iMDFjLTkwMmNiZDZmNTFhYiJ9.Q7tjaQxcprfmA_bbCDhkiHW7iEwnVDFL_iyodIO_iWgSyE_pqHXpYI0ls-lRt-QtTzP53FlFh39_dutipNWYQL9sl4yLwFhdrdXaO04gchYJD-KyorzQcUoV8O6Wvd0QGxSwSYlvcNIX9UvRcJmDEIfGTa95b4_nIGSLOiYrt5RK2SghNz1SxHgpWa5jZKdMdkqmGQIrEkXKPSpH051snHGQPwdgZcBQyhkacPKAs9UtqqoMdgBuffufFBbSpJPPCTcboi7A3PYetQ7vh7n9X-Xs04BXgkNj4qMWbmdHZFK1Bu5okulBznIpRakN_AOyMFOR0t_ACjogbuDJndfnsg' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3MzMxMDU1MDgsImlhdCI6MTczMzA2OTUwOCwianRpIjoiMjlkZTVlMTMtZTY5Ny00ZjAwLWI3ZTAtZmJhNDQ2ODE0OTU0IiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjkwMWRkNWMzLTJmZTAtNDJmMC1iZTYwLWNhYmI2OGE1MWZlMCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjIyZmQzNzI3LTMwNDQtNDU5Ni04NTUyLWY0ZTU0MjA1ZjU0MCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudElkIjoiMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50SG9zdCI6IjM0LjEwMS4yMTYuMTY4IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTYuMTY4In0.CLgxwgwAZTwt4siqYiwc5nLCW52gqtGi_c-FFpmPZsZzdqjWGfFYIeB7aOSzt5-rjWrplQJ3Xb_cXmznqNcIjpiGQ7_Qp7v1TfMhp5tkn-bR-LsOQ4bnF1muyioy6lRHqWytSDEq7orxPlsBTU4Sumwrzv_-DvLBRqQD565ZWtsPqTaByIfQcHDqqKM_pSn80T9vo5Lt5sebJ6ttM4Z7Xyir5rhO93yZLAXiz-Ih8xoOQgZawfvICxhO4UM2LgPtzijqWGo9W2XpD3M7rXEFXedxqKzJwFwXpQhWeBv__RWNDIAQ6fAEucx4QDjbZtdBAxb6u-TQrXIsCQoNwWJm8A' \
--header 'Content-Type: application/json' \
--data-raw '{
   "originalPartnerReferenceNo":"1231231123"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2006400",
    "responseMessage": "Successful",
    "originalPartnerReferenceNo": "1231231123",
    "originalReferenceNo": "INV/PAC/ONP/241201/70URMSYMQX0",
    "amount": {
        "value": "2000.00",
        "currency": "IDR"
    },
    "paidTime": "2024-12-01T23:12:52.929051",
    "latestTransactionStatus": "00",
    "transactionStatusDesc": "SUCCESS"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| amount | Object | Mandatory | - |
| amount.value | String | Mandatory | Jumlah bersih transaksi yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| amount.currency | String(ISO4217) | Mandatory | Mata Uang |
| paidTime | String | Mandatory | Tanggal transaksi *reserve* dibayarkan |
| latestTransactionStatus | String | Mandatory | 00 - SUCCESS 05 - CANCELED 06 - FAILED |
| transactionStatusDesc | String | Mandatory | Deskripsi status *reserve* |


### API Capture

API ini digunakan untuk merchant dapat melakukan *capture* pada transaksi yang telah di-*reserve* dan dapat mengurangi saldo *customer*. Setiap nomor transaksi hanya dapat di-*capture* satu kali, sisa saldo akan otomatis dikembalikan kepada *customer*.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 65 |
| Channel ID | 02165 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/auth/capture |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Capture (02165) |


### Request Body

**Contoh cURL Capture**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/auth/capture' \
--header 'X-TIMESTAMP: 2024-12-01T23:16:04+07:00' \
--header 'X-SIGNATURE: 6RQdrzmtX0DkdyM0WEQX2MF8ellBnU2fuM+MAkb+881tYiutxUq2fj4tmJmioxeqDg2iXx0HrhTZg8MdX+oJ7w==' \
--header 'X-EXTERNAL-ID: 12229470217009412823018139400021' \
--header 'X-PARTNER-ID: 22fd3727-3044-4596-8552-f4e54205f599' \
--header 'CHANNEL-ID: 02165' \
--header 'Authorization-Customer: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAwMTYwNCIsImFjY291bnRJZCI6MTUyMTYsImFjY291bnRJZFBvaW50IjoxMzYzMiwibmJmIjoxNzMzMDY5NTU0LCJjYklkIjoiZWExMWM2MTgtMDc0MC00ZDQyLTg1YWQtNjNjZWQxMjIwMzFmIiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiZXhwIjoxNzM0MzY1NTU0LCJpYXQiOjE3MzMwNjk1NTQsImp0aSI6ImFjY2I0YTBmLTg3NmEtNDVlZi1iMDFjLTkwMmNiZDZmNTFhYiJ9.Q7tjaQxcprfmA_bbCDhkiHW7iEwnVDFL_iyodIO_iWgSyE_pqHXpYI0ls-lRt-QtTzP53FlFh39_dutipNWYQL9sl4yLwFhdrdXaO04gchYJD-KyorzQcUoV8O6Wvd0QGxSwSYlvcNIX9UvRcJmDEIfGTa95b4_nIGSLOiYrt5RK2SghNz1SxHgpWa5jZKdMdkqmGQIrEkXKPSpH051snHGQPwdgZcBQyhkacPKAs9UtqqoMdgBuffufFBbSpJPPCTcboi7A3PYetQ7vh7n9X-Xs04BXgkNj4qMWbmdHZFK1Bu5okulBznIpRakN_AOyMFOR0t_ACjogbuDJndfnsg' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'Authorization: Bearer eyJhbHciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3MzMxMDU1MDgsImlhdCI6MTczMzA2OTUwOCwianRpIjoiMjlkZTVlMTMtZTY5Ny00ZjAwLWI3ZTAtZmJhNDQ2ODE0OTU0IiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjkwMWRkNWMzLTJmZTAtNDJmMC1iZTYwLWNhYmI2OGE1MWZlMCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjIyZmQzNzI3LTMwNDQtNDU5Ni04NTUyLWY0ZTU0MjA1ZjU0MCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudElkIjoiMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50SG9zdCI6IjM0LjEwMS4yMTYuMTY4IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTYuMTY4In0.CLgxwgwAZTwt4siqYiwc5nLCW52gqtGi_c-FFpmPZsZzdqjWGfFYIeB7aOSzt5-rjWrplQJ3Xb_cXmznqNcIjpiGQ7_Qp7v1TfMhp5tkn-bR-LsOQ4bnF1muyioy6lRHqWytSDEq7orxPlsBTU4Sumwrzv_-DvLBRqQD565ZWtsPqTaByIfQcHDqqKM_pSn80T9vo5Lt5sebJ6ttM4Z7Xyir5rhO93yZLAXiz-Ih8xoOQgZawfvICxhO4UM2LgPtzijqWGo9W2XpD3M7rXEFXedxqKzJwFwXpQhWeBv__RWNDIAQ6fAEucx4QDjbZtdBAxb6u-TQrXIsCQoNwWJm8A' \
--header 'Content-Type: application/json' \
--data-raw '{
   "originalPartnerReferenceNo":"1231231123",
   "originalReferenceNo":"INV/PAC/ONP/241201/70URMSYMQX0",
   "merchantId":"9653109f-2806-48cf-ad75-85664c2db33e",
   "partnerCaptureNo":"00007100010112399123",
   "captureAmount":{
      "value":"2000.00",
      "currency":"IDR"
   },
   "title":"confirm",
   "lastCapture":"TRUE"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| partnerCaptureNo | String | Mandatory | ID transaksi *capture* pada Merchant/Partner |
| captureAmount | Object | Mandatory | - |
| captureAmount.value | String | Mandatory | Jumlah transaksi *capture* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| captureAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| title | String | Mandatory | Judul *capture* |
| lastCapture | String | Mandatory | *Flag* untuk menentukan apakah ini adalah *capture* terakhir dan AstraPay akan melakukan pengembalian sisa uang *customer* ika masih ada saldo yang tersisa. Saat ini AstraPay hanya menyediakan *one time capture* sehingga hanya dapat diisi dengan "TRUE" |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2006500",
    "responseMessage": "Successful",
    "originalReferenceNo": "INV/PAC/ONP/241201/70URMSYMQX0",
    "originalPartnerReferenceNo": "1231231123",
    "partnerCaptureNo": "00007100010112399123",
    "captureNo": "C00000701733069765518",
    "captureAmount": {
        "value": "2000.00",
        "currency": "IDR"
    },
    "captureTime": "2024-12-01T23:16:05.803596"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| partnerCaptureNo | String | Mandatory | ID transaksi *capture* pada Merchant/Partner |
| CaptureNo | String | Mandatory | ID transaksi *capture* pada AstraPay |
| captureAmount | Object | Mandatory | - |
| captureAmount.value | String | Mandatory | Jumlah transaksi *capture* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| captureAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| captureTime | String | Mandatory | Tanggal transaksi *capture* dibayarkan |


### API Capture Query

API ini digunakan untuk mengecek status *capture customer*. Mandatory digunakan apabila merchant mendapatkan response "internal server error" ketika hit API Capture.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 66 |
| Channel ID | 02266 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/auth/capture-query |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Capture Query (02266) |


### Request Body

**Contoh cURL Capture Query**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/auth/capture-query' \
--header 'X-TIMESTAMP: 2024-12-01T23:17:04+07:00' \
--header 'X-SIGNATURE: hmEu2IypdrWRI7nHvyegoUmE65J9Zo8UxC47qWasrFxj2q6Foj7RFwnhjEvisv3z6k44u18R8v6uJbBDHjZ8dQ==' \
--header 'X-EXTERNAL-ID: 24959497050700540406541e' \
--header 'X-PARTNER-ID: 22fd4727-3044-4596-8552-f4e54205f540' \
--header 'CHANNEL-ID: 02266' \
--header 'Authorization-Customer: Bearer eyJ0eXBiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAwMTYwNCIsImFjY291bnRJZCI6MTUyMTYsImFjY291bnRJZFBvaW50IjoxMzYzMiwibmJmIjoxNzMzMDY5NTU0LCJjYklkIjoiZWExMWM2MTgtMDc0MC00ZDQyLTg1YWQtNjNjZWQxMjIwMzFmIiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiZXhwIjoxNzM0MzY1NTU0LCJpYXQiOjE3MzMwNjk1NTQsImp0aSI6ImFjY2I0YTBmLTg3NmEtNDVlZi1iMDFjLTkwMmNiZDZmNTFhYiJ9.Q7tjaQxcprfmA_bbCDhkiHW7iEwnVDFL_iyodIO_iWgSyE_pqHXpYI0ls-lRt-QtTzP53FlFh39_dutipNWYQL9sl4yLwFhdrdXaO04gchYJD-KyorzQcUoV8O6Wvd0QGxSwSYlvcNIX9UvRcJmDEIfGTa95b4_nIGSLOiYrt5RK2SghNz1SxHgpWa5jZKdMdkqmGQIrEkXKPSpH051snHGQPwdgZcBQyhkacPKAs9UtqqoMdgBuffufFBbSpJPPCTcboi7A3PYetQ7vh7n9X-Xs04BXgkNj4qMWbmdHZFK1Bu5okulBznIpRakN_AOyMFOR0t_ACjogbuDJndfnsg' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3MzMxMDU1MDgsImlhdCI6MTczMzA2OTUwOCwianRpIjoiMjlkZTVlMTMtZTY5Ny00ZjAwLWI3ZTAtZmJhNDQ2ODE0OTU0IiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjkwMWRkNWMzLTJmZTAtNDJmMC1iZTYwLWNhYmI2OGE1MWZlMCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjIyZmQzNzI3LTMwNDQtNDU5Ni04NTUyLWY0ZTU0MjA1ZjU0MCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudElkIjoiMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50SG9zdCI6IjM0LjEwMS4yMTYuMTY4IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTYuMTY4In0.CLgxwgwAZTwt4siqYiwc5nLCW52gqtGi_c-FFpmPZsZzdqjWGfFYIeB7aOSzt5-rjWrplQJ3Xb_cXmznqNcIjpiGQ7_Qp7v1TfMhp5tkn-bR-LsOQ4bnF1muyioy6lRHqWytSDEq7orxPlsBTU4Sumwrzv_-DvLBRqQD565ZWtsPqTaByIfQcHDqqKM_pSn80T9vo5Lt5sebJ6ttM4Z7Xyir5rhO93yZLAXiz-Ih8xoOQgZawfvICxhO4UM2LgPtzijqWGo9W2XpD3M7rXEFXedxqKzJwFwXpQhWeBv__RWNDIAQ6fAEucx4QDjbZtdBAxb6u-TQrXIsCQoNwWJm8A' \
--header 'Content-Type: application/json' \
--data-raw '{
   "originalReferenceNo":"INV/PAC/ONP/241201/70URMSYMQX0",
   "merchantId": "9653109f-2806-48cf-ad75-85664c2db33e",
   "partnerCaptureNo": "00007100010112399123"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| partnerCaptureNo | String | Mandatory | ID transaksi *capture* pada Merchant/Partner |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2006600",
    "responseMessage": "Successful",
    "originalPartnerReferenceNo": "1231231123",
    "originalReferenceNo": "INV/PAC/ONP/241201/70URMSYMQX0",
    "captureNo": "C00000701733069765518",
    "captureAmount": {
        "value": "2000.00",
        "currency": "IDR"
    },
    "captureTime": "2024-12-01T23:16:05.803596",
    "latestCaptureStatus": "SUCCESS",
    "partnerCaptureNo": "00007100010112399123"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| partnerCaptureNo | String | Mandatory | ID transaksi *capture* pada Merchant/Partner |
| CaptureNo | String | Mandatory | ID transaksi *capture* pada AstraPay |
| captureAmount | Object | Mandatory | - |
| captureAmount.value | String | Mandatory | Jumlah transaksi *capture* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| captureAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| captureTime | String | Mandatory | Tanggal transaksi *capture* dibayarkan |
| latestCaptureStatus | String | Mandatory | Status *capture* |


### API Void

API ini digunakan apabila merchant ingin mengembalikan saldo customer yang sebelumnya sudah berhasil ter-reserve.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 67 |
| Channel ID | 02367 |
| Method | POST |
| URL Sandbox | /merchant-service/v1.0/auth/void |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Void (02367) |


### Request Body

**Contoh cURL Void**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/auth/void' \
--header 'X-TIMESTAMP: 2024-12-01T23:17:04+07:00' \
--header 'X-SIGNATURE: itk3mg8+ANh2gaPXgEXi+JHek9Ag/m01x5UEnAtCUPVJwBNlO4WMnis2KwMvGlyz7fb5Ya85X1CPl8m7SYdzJQ==' \
--header 'X-EXTERNAL-ID: 2495649705070054040654162' \
--header 'X-PARTNER-ID: 22fd3727-3044-4596-8552-f4e54209f540' \
--header 'CHANNEL-ID: 02367' \
--header 'Authorization-Customer: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAwMTYwNCIsImFjY291bnRJZCI6MTUyMTYsImFjY291bnRJZFBvaW50IjoxMzYzMiwibmJmIjoxNzMzMDY5NTU0LCJjYklkIjoiZWExMWM2MTgtMDc0MC00ZDQyLTg1YWQtNjNjZWQxMjIwMzFmIiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiZXhwIjoxNzM0MzY1NTU0LCJpYXQiOjE3MzMwNjk1NTQsImp0aSI6ImFjY2I0YTBmLTg3NmEtNDVlZi1iMDFjLTkwMmNiZDZmNTFhYiJ9.Q7tjaQxcprfmA_bbCDhkiHW7iEwnVDFL_iyodIO_iWgSyE_pqHXpYI0ls-lRt-QtTzP53FlFh39_dutipNWYQL9sl4yLwFhdrdXaO04gchYJD-KyorzQcUoV8O6Wvd0QGxSwSYlvcNIX9UvRcJmDEIfGTa95b4_nIGSLOiYrt5RK2SghNz1SxHgpWa5jZKdMdkqmGQIrEkXKPSpH051snHGQPwdgZcBQyhkacPKAs9UtqqoMdgBuffufFBbSpJPPCTcboi7A3PYetQ7vh7n9X-Xs04BXgkNj4qMWbmdHZFK1Bu5okulBznIpRakN_AOyMFOR0t_ACjogbuDJndfnsg' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3MzMxMDU1MDgsImlhdCI6MTczMzA2OTUwOCwianRpIjoiMjlkZTVlMTMtZTY5Ny00ZjAwLWI3ZTAtZmJhNDQ2ODE0OTU0IiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjkwMWRkNWMzLTJmZTAtNDJmMC1iZTYwLWNhYmI2OGE1MWZlMCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjIyZmQzNzI3LTMwNDQtNDU5Ni04NTUyLWY0ZTU0MjA1ZjU0MCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudElkIjoiMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50SG9zdCI6IjM0LjEwMS4yMTYuMTY4IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTYuMTY4In0.CLgxwgwAZTwt4siqYiwc5nLCW52gqtGi_c-FFpmPZsZzdqjWGfFYIeB7aOSzt5-rjWrplQJ3Xb_cXmznqNcIjpiGQ7_Qp7v1TfMhp5tkn-bR-LsOQ4bnF1muyioy6lRHqWytSDEq7orxPlsBTU4Sumwrzv_-DvLBRqQD565ZWtsPqTaByIfQcHDqqKM_pSn80T9vo5Lt5sebJ6ttM4Z7Xyir5rhO93yZLAXiz-Ih8xoOQgZawfvICxhO4UM2LgPtzijqWGo9W2XpD3M7rXEFXedxqKzJwFwXpQhWeBv__RWNDIAQ6fAEucx4QDjbZtdBAxb6u-TQrXIsCQoNwWJm8A' \
--header 'Content-Type: application/json' \
--data-raw '{
   "originalReferenceNo":"INV/PAC/ONP/241201/70HVTUPYCW1",
   "originalPartnerReferenceNo": "63g2j28em",
   "merchantId":"9653109f-2806-48cf-ad75-85664c2db33e",
   "voidAmount":{
      "value":"10000.00",
      "currency":"IDR"
   },
   "voidRemainingAmount": "TRUE",
   "partnerVoidNo": "VOID/1092982873784"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| partnerVoidNo | String | Mandatory | ID transaksi *void* pada Merchant/Partner |
| voidAmount | Object | Mandatory | - |
| voidAmount.value | String | Mandatory | Jumlah transaksi *void* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| voidAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| reason | String | Mandatory | Alasan *void* |
| voidRemainingAmount | String | Mandatory | *Flag* untuk menentukan apakah ini adalah *void* terakhir. Saat ini AstraPay hanya menyediakan *one time void* sehingga hanya dapat diisi dengan "TRUE" |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2006700",
    "responseMessage": "Successful",
    "originalReferenceNo": "INV/PAC/ONP/241201/70HVTUPYCW1",
    "originalPartnerReferenceNo": "63g2j28em",
    "voidNo": "V00000701733071064098",
    "partnerVoidNo": "VOID/1092982873784",
    "voidAmount": {
        "value": "10000.00",
        "currency": "IDR"
    },
    "voidTime": "2024-12-01T23:37:44.274825"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| partnerVoidNo | String | Mandatory | ID transaksi *void* pada Merchant/Partner |
| voidNo | String | Mandatory | ID transaksi *void* pada AstraPay |
| voidAmount | Object | Mandatory | - |
| voidAmount.value | String | Mandatory | Jumlah transaksi *void* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| voidAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| voidTime | String | Mandatory | Tanggal transaksi *void* dibayarkan |


### API Void Query

API ini digunakan untuk mengecek status *void customer*. Mandatory digunakan apabila merchant mendapatkan response "internal server error" ketika hit API Void.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 68 |
| Channel ID | 02468 |
| Method | POST |
| URL Sandbox | /merchant-service/snap/v1.0/auth/void-query |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik customer hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Void Query (02468) |


### Request Body

**Contoh cURL Void Query**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/auth/void-query' \
--header 'X-TIMESTAMP: 2024-12-01T23:17:04+07:00' \
--header 'X-SIGNATURE: 7fvrP+x4x8LZlAXqoF6FOqM2myGF4BBDbpOyzZIvGlb1nIieTrEsov3lmWZovmsjR8xlflWBKTcX+Dy+oBRzTw==' \
--header 'X-EXTERNAL-ID: 2495649705079954040654980' \
--header 'X-PARTNER-ID: 22fd&727-3044-4596-8552-f4e54205f540' \
--header 'CHANNEL-ID: 02468' \
--header 'Authorization-Customer: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAwMTYwNCIsImFjY291bnRJZCI6MTUyMTYsImFjY291bnRJZFBvaW50IjoxMzYzMiwibmJmIjoxNzMzMDY5NTU0LCJjYklkIjoiZWExMWM2MTgtMDc0MC00ZDQyLTg1YWQtNjNjZWQxMjIwMzFmIiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiZXhwIjoxNzM0MzY1NTU0LCJpYXQiOjE3MzMwNjk1NTQsImp0aSI6ImFjY2I0YTBmLTg3NmEtNDVlZi1iMDFjLTkwMmNiZDZmNTFhYiJ9.Q7tjaQxcprfmA_bbCDhkiHW7iEwnVDFL_iyodIO_iWgSyE_pqHXpYI0ls-lRt-QtTzP53FlFh39_dutipNWYQL9sl4yLwFhdrdXaO04gchYJD-KyorzQcUoV8O6Wvd0QGxSwSYlvcNIX9UvRcJmDEIfGTa95b4_nIGSLOiYrt5RK2SghNz1SxHgpWa5jZKdMdkqmGQIrEkXKPSpH051snHGQPwdgZcBQyhkacPKAs9UtqqoMdgBuffufFBbSpJPPCTcboi7A3PYetQ7vh7n9X-Xs04BXgkNj4qMWbmdHZFK1Bu5okulBznIpRakN_AOyMFOR0t_ACjogbuDJndfnsg' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3MzMxMDU1MDgsImlhdCI6MTczMzA2OTUwOCwianRpIjoiMjlkZTVlMTMtZTY5Ny00ZjAwLWI3ZTAtZmJhNDQ2ODE0OTU0IiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjkwMWRkNWMzLTJmZTAtNDJmMC1iZTYwLWNhYmI2OGE1MWZlMCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjIyZmQzNzI3LTMwNDQtNDU5Ni04NTUyLWY0ZTU0MjA1ZjU0MCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudElkIjoiMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50SG9zdCI6IjM0LjEwMS4yMTYuMTY4IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTYuMTY4In0.CLgxwgwAZTwt4siqYiwc5nLCW52gqtGi_c-FFpmPZsZzdqjWGfFYIeB7aOSzt5-rjWrplQJ3Xb_cXmznqNcIjpiGQ7_Qp7v1TfMhp5tkn-bR-LsOQ4bnF1muyioy6lRHqWytSDEq7orxPlsBTU4Sumwrzv_-DvLBRqQD565ZWtsPqTaByIfQcHDqqKM_pSn80T9vo5Lt5sebJ6ttM4Z7Xyir5rhO93yZLAXiz-Ih8xoOQgZawfvICxhO4UM2LgPtzijqWGo9W2XpD3M7rXEFXedxqKzJwFwXpQhWeBv__RWNDIAQ6fAEucx4QDjbZtdBAxb6u-TQrXIsCQoNwWJm8A' \
--header 'Content-Type: application/json' \
--data-raw '{
   "originalReferenceNo":"INV/PAC/ONP/241201/70HVTUPYCW1",
   "originalPartnerReferenceNo": "63g2j28em",
   "merchantId":"9653109f-2806-48cf-ad75-85664c2db33e",
   "partnerVoidNo": "VOID/1092982873784"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| merchantId | String | Mandatory | Kode unik setiap merchant yang diberikan AstraPay |
| partnerVoidNo | String | Mandatory | ID transaksi *void* pada Merchant/Partner |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2006800",
    "responseMessage": "Successful",
    "originalPartnerReferenceNo": "63g2j28em",
    "originalReferenceNo": "INV/PAC/ONP/241201/70HVTUPYCW1",
    "voidNo": "V00000701733071064098",
    "voidAmount": {
        "value": "10000.00",
        "currency": "IDR"
    },
    "voidTime": "2024-12-01T23:37:44.274825",
    "latestVoidStatus": "SUCCESS",
    "partnerVoidNo": "VOID/1092982873784"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| partnerVoidNo | String | Mandatory | ID transaksi *void* pada Merchant/Partner |
| voidNo | String | Mandatory | ID transaksi *void* pada AstraPay |
| voidAmount | Object | Mandatory | - |
| voidAmount.value | String | Mandatory | Jumlah transaksi *void* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| voidAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| voidTime | String | Mandatory | Tanggal transaksi *void* dibayarkan |
| latestVoidStatus | String | Mandatory | Status *void* |


### API Auth Refund

API ini digunakan untuk melakukan pengembalian saldo kepada *customer* atas transaksi Auth Payment.

### Protocol & Service Address


| Item | Value |
| --- | --- |
| Protocol | HTTPS |
| Service Code | 69 |
| Channel ID | 02569 |
| Method | POST |
| URL Sandbox | merchant-service/snap/v1.0/auth/refund |
| Content-Type | application/json |


### Request Header


| Name | Type | Requirement | Description |
| --- | --- | --- | --- |
| Content-Type | String | Mandatory | Tipe konten, data yang dikirim harus selalu application/json |
| Authorization | String | Mandatory | Bearer token hasil generate dari [**API Access Token B2B**](#api-access-token-b2b) |
| Authorization-Customer | String | Mandatory | Akses token milik *customer* hasil generate dari [**API Access Token B2B2C**](#api-access-token-b2b2c) |
| X-TIMESTAMP | String | Mandatory | Waktu lokal Merchant/Partner dalam format **yyyy-MM-ddTHH:mm:ssTZD** |
| X-SIGNATURE | String | Mandatory | Hasil dari generate Signature Service |
| X-PARTNER-ID | String | Mandatory | Client ID Merchant/Partner yang didapat dari AstraPay |
| X-EXTERNAL-ID | String | Mandatory | Numeric string unik yang hanya dapat digunakan satu kali dalam satu hari. Format yang digunakan adalah: 36 Random Numeric String |
| X-DEVICE-ID | String | Mandatory | Identifier device yang digunakan oleh customer pada API yang sedang diakses |
| CHANNEL-ID | String | Mandatory | ID dari service yang mengakses API Auth Refund (02569) |


### Request Body

**Contoh cURL Auth Refund**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/snap/v1.0/auth/refund' \
--header 'X-TIMESTAMP: 2025-01-01T23:12:51+07:00' \
--header 'X-SIGNATURE: qgmpONHPt/5u54tRYf8w3nuBzvqX7wjAj5dfyWqgPrGABFVHvETxW22kNqiLDNGr8zI3H+qpXNiQHo1yMLLeiw==' \
--header 'X-EXTERNAL-ID: 47941397849362225487741579324988' \
--header 'X-PARTNER-ID: 22fd3727-3044-4596-8552-f4e54205f599' \
--header 'CHANNEL-ID: 02569' \
--header 'Authorization-Customer: Bearer eyJ0eYAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiIyNTAwMTYwNCIsImFjY291bnRJZCI6MTUyMTYsImFjY291bnRJZFBvaW50IjoxMzYzMiwibmJmIjoxNzMzMDY5NTU0LCJjYklkIjoiZWExMWM2MTgtMDc0MC00ZDQyLTg1YWQtNjNjZWQxMjIwMzFmIiwiaXNzIjoiQXN0cmFQYXktRGV2IiwiY2xhaW0iOiJTTkFQIiwiZXhwIjoxNzM0MzY1NTU0LCJpYXQiOjE3MzMwNjk1NTQsImp0aSI6ImFjY2I0YTBmLTg3NmEtNDVlZi1iMDFjLTkwMmNiZDZmNTFhYiJ9.Q7tjaQxcprfmA_bbCDhkiHW7iEwnVDFL_iyodIO_iWgSyE_pqHXpYI0ls-lRt-QtTzP53FlFh39_dutipNWYQL9sl4yLwFhdrdXaO04gchYJD-KyorzQcUoV8O6Wvd0QGxSwSYlvcNIX9UvRcJmDEIfGTa95b4_nIGSLOiYrt5RK2SghNz1SxHgpWa5jZKdMdkqmGQIrEkXKPSpH051snHGQPwdgZcBQyhkacPKAs9UtqqoMdgBuffufFBbSpJPPCTcboi7A3PYetQ7vh7n9X-Xs04BXgkNj4qMWbmdHZFK1Bu5okulBznIpRakN_AOyMFOR0t_ACjogbuDJndfnsg' \
--header 'X-DEVICE-ID: 09864ADCASA' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE3MzMxMDU1MDgsImlhdCI6MTczMzA2OTUwOCwianRpIjoiMjlkZTVlMTMtZTY5Ny00ZjAwLWI3ZTAtZmJhNDQ2ODE0OTU0IiwiaXNzIjoiaHR0cHM6Ly9rZXljbG9hay11YXQuYXN0cmFwYXkuY29tL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjkwMWRkNWMzLTJmZTAtNDJmMC1iZTYwLWNhYmI2OGE1MWZlMCIsInR5cCI6IkJlYXJlciIsImF6cCI6IjIyZmQzNzI3LTMwNDQtNDU5Ni04NTUyLWY0ZTU0MjA1ZjU0MCIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImNsaWVudElkIjoiMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50SG9zdCI6IjM0LjEwMS4yMTYuMTY4IiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJzZXJ2aWNlLWFjY291bnQtMjJmZDM3MjctMzA0NC00NTk2LTg1NTItZjRlNTQyMDVmNTQwIiwiY2xpZW50QWRkcmVzcyI6IjM0LjEwMS4yMTYuMTY4In0.CLgxwgwAZTwt4siqYiwc5nLCW52gqtGi_c-FFpmPZsZzdqjWGfFYIeB7aOSzt5-rjWrplQJ3Xb_cXmznqNcIjpiGQ7_Qp7v1TfMhp5tkn-bR-LsOQ4bnF1muyioy6lRHqWytSDEq7orxPlsBTU4Sumwrzv_-DvLBRqQD565ZWtsPqTaByIfQcHDqqKM_pSn80T9vo5Lt5sebJ6ttM4Z7Xyir5rhO93yZLAXiz-Ih8xoOQgZawfvICxhO4UM2LgPtzijqWGo9W2XpD3M7rXEFXedxqKzJwFwXpQhWeBv__RWNDIAQ6fAEucx4QDjbZtdBAxb6u-TQrXIsCQoNwWJm8A' \
--header 'Content-Type: application/json' \
--data-raw '{
   "originalPartnerReferenceNo":"1231231123",
   "partnerRefundNo":"RFN123123",
   "refundAmount":{
      "value":"2000.00",
      "currency":"IDR"
   },
   "reason":"Customer Complain",
   "originalCaptureNo":"C00000701733069765518"
}'
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| partnerRefundNo | String | Mandatory | ID *refund* pada Merchant/Partner |
| refundAmount | Object | Mandatory | - |
| refundAmount.value | String | Mandatory | Jumlah transaksi *refund* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| refundAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| reason | String | Optional | Alasan pengembalian saldo |
| originalCaptureNo | String | Mandatory | ID transaksi *capture* pada AstraPay |


### Response Body

**Contoh Response**

```shell
{
    "responseCode": "2006900",
    "responseMessage": "Successful",
    "originalPartnerReferenceNo": "1231231123",
    "originalReferenceNo": "INV/PAC/ONP/241201/70URMSYMQX0",
    "refundNo": "INV/REF/ONP/240422/505625CAC",
    "partnerRefundNo": "RFN123123",
     "refundAmount": {
        "value": "2000.00",
        "currency": "IDR"
    },
    "refundTime": "2025-01-01T23:12:52.929051",
    "originalCaptureNo": "C00000701733069765518"
}
```


| Field | Type | Requirement | Description |
| --- | --- | --- | --- |
| responseCode | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| responseMessage | String | Mandatory | [**Response List Payment Channel**](#response-list-payment-channel) |
| originalPartnerReferenceNo | String | Mandatory | ID transaksi *reserve* pada Merchant/Partner |
| originalReferenceNo | String | Mandatory | ID transaksi *reserve* pada AstraPay |
| refundNo | String | Mandatory | ID *refund* pada AstraPay |
| partnerRefundNo | String | Mandatory | ID *refund* pada Merchant/Partner |
| refundAmount | Object | Mandatory | - |
| refundAmount.value | String | Mandatory | Jumlah transaksi *refund* yang diterima termasuk 2 digit desimal. **Cth: 10000.00** |
| refundAmount.currency | String(ISO4217) | Mandatory | Mata Uang |
| refundTime | String | Mandatory | Waktu pengembalian saldo |
| originalCaptureNo | String | Mandatory | ID transaksi *capture* pada AstraPay |


## Response List Payment Channel



Dibawah ini merupakan daftar response code yang akan muncul. Format “xx” dalam Response Code diisikan dengan Service Code dari masing-masing api.


| Response Code | Response Message | Description |
| --- | --- | --- |
| 200xx00 | Successful | Sukses |
| 400xx00 | Bad Request | Pesan umum yang akan diterima ketika permintaan gagal/tidak sesuai |
| 400xx01 | Invalid Field Format CHANNEL-ID | Format field CHANNEL-ID salah |
| 400xx02 | Invalid Mandatory Field Format X-TIMESTAMP | X-TIMESTAMP tidak ada atau tidak lengkap |
| 400xx02 | Invalid Mandatory Field Format X-EXTERNAL-ID | X-EXTERNAL-ID tidak ada atau tidak lengkap |
| 400xx02 | Invalid Mandatory Field Format X-PARTNER-ID | X-PARTNER-ID tidak ada atau tidak lengkap |
| 401xx00 | Unauthorized. Signature | Invalid signature |
| 401xx01 | Invalid Token (B2B) | Request token invalid |
| 404xx01 | Transaction Not Found | Nomor transaksi tidak tercatat pada AstraPay |
| 404xx08 | Invalid Merchant | Client ID atau Merchant ID tidak ditemukan |
| 409xx00 | Conflict | Menggunakan X-EXTERNAL-ID yang sama di hari yang sama |
| 500xx00 | General Error | General Error |
| 500xx01 | Internal Server Error | Kesalahan Server Internal, harap periksa status transaksi |


### Response List Account Unbinding


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4010904 | Customer Token Not Found | Token tidak ditemukan pada sistem |
| 4030906 | Feature Not Allowed At This Time. Pending Auth Transaction | Permintaan tidak dapat diproses karena terdapat transaksi auth payment yang masih berstatus RESERVED dan belum CAPTURED/VOID |


### Response List Account Binding Inquiry


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4010804 | Customer Token Not Found | Token tidak ditemukan pada sistem |


### Response List Payment


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4015402 | Invalid Token (B2B2C) | Request Token B2B2C invalid |
| 4035405 | Inactive Account | Akun Inactive |
| 4035415 | Transaction Not Permitted.Date limit exceeded | ValidUpto melebihi batas yang diperbolehkan. Min: 3 menit, Max: 15 menit |
| 4095401 | Duplicate partnerReferenceNo | Transaction ID merchant lebih dari satu |


### Response List Payment Status


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4045501 | Transaction Not Found | Transaksi tidak ditemukan |


### Response List Payment Refund


| Response Code | Response Message | Description |
| --- | --- | --- |
| 2025800 | Refund still in process | Refund sedang dalam proses |
| 4035802 | Exceeds Transaction Amount Limit | Nominal refund yang diajukan melebihi nominal transaksi (Sesuai ketentuan AstraPay) |
| 4035814 | Insufficient Funds | Partner tidak memiiki dana untuk proses refund |
| 4035815 | Transaction not permitted. Cashback balance is exist | Cashback atas transaksi tersebut sedang dalam proses |
| 4035815 | Transaction not permitted. Date limit exceeded | Tanggal transaksi > 3 bulan |
| 4035815 | Transaction not permitted. Disputed transaction | Transaksi *dispute* dan menunggu konfirmasi manual |
| 4035823 | Account Limit Exceeded | Akun customer terkena limit (daily/monthly) atau limit saldo. Baca selengkapnya mengenai limit akun customer [disini](./assets/terms-and-conditions). |
| 4045800 | Invalid Transaction Status | Status transaksi tidak *eligible* untuk dilakukan refund. Hanya transaksi dengan status 'success' dan 'partial refund' yang dapat dilakukan refund. Silakan cek status transaksi melalui response API Direct Debit Payment Status |
| 4045813 | Invalid Amount | Jumlah refund tidak sesuai dengan nilai yang seharusnya |


### Response List Balance Inquiry


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4011102 | Invalid Token B2B2C | Request token B2B2C invalid |
| 4041111 | Invalid Account | User Account is closed |


### Response List Auth Payment


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4006300 | Bad Request | Minimum amount Rp 1 |
| 4016302 | Invalid Token (B2B2C) | Access token B2B2C *invalid/expired* |
| 4036314 | Insufficient Funds | Saldo *customer* tidak cukup |
| 4036315 | Transaction Not Permitted.Date limit exceeded | ValidUpto melebihi batas yang diperbolehkan. Min: 1 menit, Max: 90 hari sejak createdAt |
| 4036315 | Transaction Not Permitted.Amount less than required fee | Amount kurang dari fee yang seharusnya diterima AstraPay |
| 4046318 | Inconsistent Request | PartnerReferenceNumber diisi dengan *amount* yang lebih kecil (status reserved sebelumnya sudah berhasil) |


### Response List Payment Query


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4016402 | Invalid Token (B2B2C) | Access token B2B2C *Expired* |


### Response List Capture


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4006500 | Bad Request | Minimum amount Rp 1 |
| 4016502 | Invalid Token (B2B2C) | Access token B2B2C *expired* |
| 4036502 | Exceeds Transaction Amount Limit | originalPartnerReferenceNumber diisi dengan *amount* yang lebih besar dibandingkan dengan amount *reserve* |
| 4036515 | Transaction Not Permitted.Amount less than required fee | Amount kurang dari fee yang seharusnya diterima AstraPay |
| 4046500 | Invalid Transaction Status | Transaksi belum berhasil ter-*reserve* sehingga tidak dapat dibatalkan |
| 4046518 | Inconsistent Request | originalPartnerReferenceNumber dan originalReferenceNumber tidak *match* |


### Response List Capture Query


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4016602 | Invalid Token (B2B2C) | Access token B2B2C *expired* |
| 4046618 | Inconsistent Request | originalPartnerReferenceNumber and partnerCaptureNo tidak *match* |


### Response List Void


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4016702 | Invalid Token (B2B2C) | Access token B2B2C *expired* |
| 4036702 | Exceeds Transaction Amount Limit | originalPartnerReferenceNumber diisi dengan *amount* yang tidak sama dengan amount *reserve* |
| 4046700 | Invalid Transaction Status | Transaksi belum berhasil ter-*reserve* sehingga tidak dapat dibatalkan |
| 4046718 | Inconsistent Request | originalPartnerReferenceNumber dan originalReferenceNumber tidak *match* |


### Response List Void Query


| Response Code | Response Message | Description |
| --- | --- | --- |
| 4016802 | Invalid Token (B2B2C) | Access token B2B2C *expired* |
| 4046818 | Inconsistent Request | originalPartnerReferenceNumber and originalReferenceNumber tidak *match* |


### Response List Auth Refund


| Response Code | Response Message | Description |
| --- | --- | --- |
| 2026900 | Refund still in process | Refund sedang dalam proses |
| 4016902 | Invalid Token B2B2C | Access token B2B2C *invalid/expired* |
| 4036902 | Exceeds Transaction Amount Limit | Nominal refund yang diajukan melebihi nominal transaksi (Sesuai ketentuan AstraPay) |
| 4036914 | Insufficient Funds | Partner tidak memiiki dana untuk proses refund |
| 4036915 | Transaction not permitted. Cashback balance is exist | Cashback atas transaksi tersebut sedang dalam proses |
| 4036915 | Transaction not permitted. Date limit exceeded | Tanggal transaksi > 3 bulan |
| 4036915 | Transaction not permitted. Disputed transaction | Transaksi *dispute* dan menunggu konfirmasi manual |
| 4036923 | Account Limit Exceeded | Akun customer terkena limit (daily/monthly) atau limit saldo. Baca selengkapnya mengenai limit akun customer [disini](./assets/terms-and-conditions) |
| 4046900 | Invalid Transaction Status | Status transaksi tidak *eligible* untuk dilakukan refund. Hanya transaksi dengan status 'success' yang dapat dilakukan refund |
| 4046918 | Inconsistent Request | originalPartnerReferenceNo dan originalCaptureNo tidak *match* |


## Panduan Penyesuaian SNAP AstraPay



Merchant yang sudah pernah melakukan integrasi dengan AstraPay dan ingin melakukan penyesuaian dengan menggunapan SNAP API AstraPay, perlu mengetahui beberapa hal dibawah ini:

### Penambahan API yang akan diimplementasi untuk melakukan integrasi sesuai SNAP


| Nama API | Deskripsi |
| --- | --- |
| [**API Access Token B2B**](#api-access-token-b2b) | Digunakan untuk mengambil token otoriasasi `Client ID` dan `Client Secret`. Token digunakan untuk otorisasi `HTTP Header` |
| [**API Access Token B2B2C**](#api-access-token-b2b2c) | Digunakan untuk mengambil token otoriasasi user sebagai identifikasi pada setiap API yang berkaitan dengan data customer |
| [**API Account Unbinding**](#api-account-unbinding) | Digunakan untuk memutuskan binding antara akun pengguna layanan (Merchant) dengan akun penyedia layanan (AstraPay) |
| [**API Account Binding Inquiry (Opsional)**](#api-account-binding-inquiry) | Digunakan untuk mengecek status binding customer |


### Daftar Perubahan API sesuai SNAP AstraPay


| Fungsi | API Sebelumnya | SNAP AstraPay |
| --- | --- | --- |
| Menghubungkan account customer dengan AstraPay | API Account Link and Registration | - [**API Account Binding**](#api-account-binding) - [**API Access Token B2B2C**](#api-access-token-b2b2c) - [**API Account Binding Inquiry (Opsional)**](#api-account-binding-inquiry) |
| Menampilkan saldo AstraPay customer | API Get Profile | [**API Balance Inquiry**](#api-balance-inquiry) |
| Memproses pembayaran | API Payment with Linking API Push to Payment | [**API Direct Debit Payment**](#api-direct-debit-payment) |
| Melakukan pengembalian dana customer | API Refund | [**API Direct Debit Refund**](#api-refund) |
| Mengetahui status transaksi yang sedang di proses | API Transaction Status | [**API Direct Debit Payment Status**](#api-direct-debit-payment-status) |
| Mendapatkan callback proses transaksi dari AstraPay | *Sebelumnya sudah terdapat di dalam API Payment with Linking dan Push to Payment* | [**API Direct Debit Nofify**](#api-direct-debit-payment-status) |

