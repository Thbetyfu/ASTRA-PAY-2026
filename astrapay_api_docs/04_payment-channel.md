# Payment Channel


Selamat datang di dokumentasi AstraPay Payment Channel, pada bagian ini akan dijelaskan cara Merchant melakukan integrasi AstraPay sebagai metode pembayaran pada Aplikasi Merchant, ada beberapa API yang kami sediakan untuk Merchant, yaitu:

1. Authorization
2. Account Link
3. Account Registration
4. Payment with linking
5. Push to Payment BETA
6. Profile
7. Transaction Status
8. Refund
9. Response Code
10. User Acceptance Testing

Dari proses di atas, akan dijelaskan apa saja yang perlu disiapkan, bagaimana step by step integration, dan apa yang akan ditampilkan ke user.

API Authorization, Payment, Profile dan Transaction Status, bisa Anda coba menggunakan Postman. Silahkan klik tombol dibawah ini untuk mencoba.



    (function (p,o,s,t,m,a,n) {
      !p[s] && (p[s] = function () { (p[t] || (p[t] = [])).push(arguments); });
      !o.getElementById(s+t) && o.getElementsByTagName("head")[0].appendChild((
        (n = o.createElement("script")),
        (n.id = s+t), (n.async = 1), (n.src = m), n
      ));
    }(window, document, "_pm", "PostmanRunObject", "https://run.pstmn.io/button.js"));
  

Sebelum melakukan proses integrasi, ada beberapa hal yang perlu dipersiapkan dan akan dibahas dibawah ini.

## 1. Introduction

### 1.1 Glosarium

Sebelum melakukan integrasi, mari kita bahas terlebih dahulu beberapa definisi dari istilah yang akan muncul pada dokumentasi ini. Penjelasan dari istilah tersebut adalah sebagai berikut:


| Istilah | Deskripsi |
| --- | --- |
| Merchant | Pihak Ketiga yang ingin melakukan integrasi dengan AstraPay |
| API | Application Programming Interface |
| Database | Kumpulan data yang telah terorganisasi dan terstruktur. |
| User | Pengguna atau Customer Jasa |
| AstraPay Classic User | User AstraPay yang telah selesai melakukan registrasi. |
| AstraPay Preferred User | User AstraPay yang telah selesai melakukan registrasi dan telah melakukan KYC di aplikasi AstraPay. |


### 1.2 Pendaftaran Merchant

**Data Sample**

```shell
Nama Merchant: Merchant
Email Merchant: [email protected]
Account Link Callback URL: merchant.com/account-link/callback
Payment Callback URL: merchant.com/payment/callback
Account Link Default URL: merchant.com/redirect/account-link/success, merchant.com/redirect/account-link/failed, merchant.com/redirect/account-link/error
Registration Default URL: merchant.com/redirect/registration/success, merchant.com/redirect/registration/failed, merchant.com/redirect/registration/error
Payment Default URL: merchant.com/redirect/payment/success, merchant.com/redirect/payment/failed, merchant.com/redirect/payment/error
```

Untuk melakukan integrasi dengan AstraPay, merchant diharuskan untuk mengirimkan beberapa data kepada AstraPay melalui email ke payment.channel@astrapay.com dengan subjek "Pendaftaran Merchant Payment Channel [Nama Merchant]", data yang dikirimkan adalah sebagai berikut:

[
 Download Formulir Pendaftaran
](./assets/Formulir_Pendaftaran_Merchant_AstraPay_1.docx)


| Field | Description | Requirement |
| --- | --- | --- |
| Nama Merchant | Nama untuk menandakan Merchant yang melakukan integrasi | Mandatory |
| Email Merchant | Email Merchant yang dapat dihubungi oleh AstraPay | Mandatory |
| Account Link Callback URL | URL Merchant yang bisa dihit oleh AstraPay ketika ingin melakukan callback ke merchant setelah proses Account Link (Section 3) maupun Registration. Authorization: No Auth. | Mandatory |
| Payment Callback URL | URL Merchant yang bisa dihit oleh AstraPay ketika ingin melakukan callback ke merchant setelah proses Payment. Authorization: No Auth. | Mandatory |
| Account Link Default URL | URL Redirect Merchant yang akan mendirect User kembali ke halaman Merchant ketika sudah selesai melakukan Account Link (Section 3). Ada 3 URL yang diberikan, yaitu untuk case Success, Failed, dan Error. | Mandatory |
| Registration Default URL | URL Redirect Merchant yang akan mendirect User kembali ke halaman Merchant ketika sudah selesai melakukan Registration (Section 3). Ada 3 URL yang diberikan, yaitu untuk case Success, Failed, dan Error. | Mandatory |
| Payment Default URL | URL Redirect Merchant yang akan mendirect User kembali ke halaman Merchant ketika sudah selesai melakukan Payment (Section 4). Ada 3 URL yang diberikan, yaitu untuk case Success, Failed, dan Error. | Mandatory |


> [!WARNING]
> Mohon pastikan untuk Callback URL yang didaftarkan oleh Merchant tidak memiliki Authorization

Setelah memberikan data tersebut ke pada AstraPay, Merchant akan diberikan credential (Mandatory) yang diperlukan untuk login Merchant Dashboard dan proses integrasi selanjutnya, credential yang diberikan adalah sebagai berikut:

**Data Sample**

```shell
Username: Merchant
Password: 23nds9&*w
ClientID: cd8a2deb-b004-4255-9c30-91656dc7d755
ClientSecret: ece0ae05-9a2e-4365-b59b-8ee7b1d8443b
```


| Field | Description |
| --- | --- |
| Username | Username Merchant yang digunakan untuk login pada Merchant Dashboard **(Coming Soon)** |
| Password | Password Merchant untuk melakukan login pada Merchant Dashboard **(Coming Soon)** |
| ClientID | String unik untuk menandakan Merchant yang mengirim request |
| ClientSecret | String unik rahasia untuk menandakan Merchant yang mengirim request |


Selain data di atas, AstraPay akan memberikan cara melakukan decrypt callbackSecurity yang nanti dibutuhkan oleh Merchant pada section 4.6.

## 2. Authorization

Authorization merupakan sebuah string token yang akan dipakai sebagai Request Header pada proses integrasi nanti, namun untuk mendapatkan Authorization ini Merchant perlu melakukan proses generate token. Proses Generate Token akan dijelaskan di bawah ini.

### 2.1 Endpoint

**Complete Code**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/api/oauth/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'grant_type=client_credentials' \
--data-urlencode 'client_id=cd8a2deb-b004-4255-9c30-91656dc7d755' \
--data-urlencode 'client_secret=ece0ae05-9a2e-4365-b59b-8ee7b1d8443b'
```

Endpoint untuk proses Generate Token memiliki URL seperti di bawah ini:

Protocol: HTTPS


Method: POST


URL Sandbox: https://sandbox.astrapay.com/api/oauth/token



### 2.2 Request Header

Request Header yang harus dimasukkan untuk hit API Authorization adalah sebagai berikut:

Content-Type: application/x-www-form-urlencoded



### 2.3 Request Body

Request Body yang harus dimasukkan untuk melakukan hit pada Endpoint Generate Token adalah sebagai berikut

**Sample Request Body**

```shell
grant_type: client_credentials
  client_id: cd8a2deb-b004-4255-9c30-91656dc7d755
  client_secret: ece0ae05-9a2e-4365-b59b-8ee7b1d8443b
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| grant_type | Validasi jenis akses. Format: client_credentials | String | Mandatory |
| client_id | String unik sebagai ID untuk menandakan Merchant yang mengirim request  (Diberikan oleh AstraPay sesuai 1.2) | String (36) | Mandatory |
| client_secret | String unik rahasia untuk menandakan Merchant yang mengirim request (Diberikan oleh AstraPay sesuai 1.2) | String (36) | Mandatory |


### 2.4 Response Body

Setelah melakukan request terhadap endpoint Generate Token, maka Merchant akan mendapat respon seperti di samping.

**Sample Response Body**

```shell
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q",
    "expires_in": 900,
    "refresh_expires_in": 0,
    "token_type": "Bearer",
    "not-before-policy": 0,
    "scope": "profile email"
}
```


| Field | Description | Type |
| --- | --- | --- |
| access_token | Token Authorization yang akan digunakan Merchant untuk menggunakan API Payment Channel AstraPay | Text |
| expires_in | Durasi berlaku token. Satuan = Detik, untuk waktu durasi token adalah 900 detik | Integer |
| refresh_expires_in | Waktu refresh kadaluarsa Token. Default: 0 | Integer |
| token_type | Default: Bearer | String |
| not-before-policy | Default: 0 | Integer |
| scope | Application scope yang diizinkan. Default: "profile email" | String |


access_token ini akan dijadikan Authorization pada proses selanjutnya, ketika token sudah kadaluarsa maka Merchant harus melakukan ulang Generate Token kembali.

## 3. Account Link and Registration

Proses Account Link merupakan proses integrasi akun AstraPay yang dimiliki oleh User dengan Aplikasi Merchant. Pada saat proses Account Link, User juga dapat melakukan Registration jika belum mempunyai akun. Setelah selesai proses Registration, maka account yang baru saja diregister juga langsung terintegrasi dengan Merchant. Berikut ini akan dijelaskan apa saja yang dibutuhkan dan bagaimana step by step proses integrasi yang perlu dilakukan.

> [!WARNING]
> Berikut yang perlu diperhatikan ketika melakukan Account Link : 
> 
> 1. Jika user Astrapay belum pernah melakukan linking dengan merchant user id yang baru, maka user wajib memasukkan PIN dan OTP.
> 
> 2. Jika user Astrapay sudah pernah melakukan linking dengan merchant user id yang sama, maka user hanya perlu memasukkan PIN saja.
> 
> 3. Jika user Astrapay sudah pernah melakukan linking dengan merchant user id yang berbeda (Satu akun astrapay untuk multiple user merchant id), maka user hanya perlu memasukkan PIN saja.

Global Architecture Account Link
![img](./assets/Payment_Flow_Diagram_Linking-33bb0e67_1.png)

Global Architecture Registration
![](./assets/Payment_Flow_Diagram_Registration-c99f54bf_1.png)

### 3.1 Endpoint

--

**Complete Code**

```shell
curl --location --request GET 'https://sandbox.astrapay.com/merchant-service/link?clientId=cd8a2deb-b004-4255-9c30-91656dc7d755&merchantUserId=M-01'
```

Endpoint untuk proses Account Link dan Registration memiliki URL yang sama seperti di bawah ini:

Protocol: HTTPS


Method: GET


Sandbox URL: https://sandbox.astrapay.com/merchant-service/link

Namun yang membedakan adalah flow yang dilakukan oleh user yang akan dijelaskan masing-masing di poin 3.4 dan 3.5.

### Request Parameter

**Sample Request Parameter**

```shell
https://sandbox.astrapay.com/merchant-service/link?clientId=cd8a2deb-b004-4255-9c30-91656dc7d755&merchantUserId=M-01
```

**Sample URL**

```shell
https://www.merchant.com/account-link/success?merchantUserId=M-01

atau

https://www.merchant.com/registration/success?merchantUserId=M-01
```

**Sample App Deeplink**

```shell
merchantapp://app/account-link/success?merchantUserId=M-01

atau

merchantapp://app/registration/success?merchantUserId=M-01
```

Request Parameter yang Merchant harus masukkan saat melakukan ujicoba adalah sebagai berikut


| Parameter | Description | Type | Requirement |
| --- | --- | --- | --- |
| clientId | String unik untuk menandakan Merchant yang mengirim request (Diberikan oleh AstraPay sesuai 1.2) | String (36) | Mandatory |
| merchantUserId | String unik untuk menandakan Akun User milik Merchant yang akan diintegrasikan dengan Akun AstraPay | String (255) | Mandatory |


Setelah memasukkan clientId pada Request Parameter, maka link akan terlihat seperti di samping. Lalu Merchant dapat langsung melakukan request tanpa perlu menambah Request Header maupun Request Body. Setelah melakukan request maka Merchant langsung diredirect ke HTML atau halaman Account Link dan Registration seperti pada 3.4 dan 3.5.

Ketika proses Account Link atau Registration selesai, maka Merchant akan diredirect sesuai dengan Account Link/Registration Default URL yang telah didaftarkan Merchant pada poin 1.2, dan akan berbentuk link seperti disamping.


| Field | Description | Type |
| --- | --- | --- |
| merchantUserId | String unik untuk menandakan Akun User milik Merchant yang akan diintegrasikan dengan Akun AstraPay | String (255) |


Di saat yang bersamaan, Sistem AstraPay akan melakukan callback ke Account Link Callback URL.



### 3.2 Account Link Callback

> [!WARNING]
> Mohon pastikan untuk URL Callback yang didaftarkan oleh Merchant tidak memiliki Authorization. Selain itu AstraPay hanya melakukan Callback ketika proses berhasil, jika gagal maka AstraPay tidak akan melakukan Callback kepada Merchant.

Ketika proses Account Link maupun Registration Sukses, maka Sistem AstraPay akan mengirimkan callback ke Account Link Callback URL Merchant yang telah didaftarkan oleh Merchant pada section 1. Adapun Request Header dan Request Body yang dikirimkan adalah sebagai berikut:

Method: POST


Content-Type: application/json

Request Body

**Sample Request Body**

```shell
{
  "signature": "b0950d6cdb9ba3f59a2baaa7e4a9cc6296b09d0a"
  "merchantUserId": "M-01"
  "callbackTimestamp": "2021-08-31 08:25:48.363"
}
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| signature | String unik untuk menandakan integrasi antara akun AstraPay (account) dengan akun Merchant (merchantUserId) | String (255) | Mandatory |
| merchantUserId | String unik untuk menandakan Akun User milik Merchant yang akan diintegrasikan dengan Akun AstraPay | String (255) | Mandatory |
| callbackTimestamp | Waktu terjadinya pengiriman callback. (Generated Automatic by AstraPay) Format: yyyy-MM-dd HH:mm:ss.SSS | String | Mandatory |


Setelah Request Callback selesai maka Merchant diharapkan memberikan response seperti di samping:

**Sample Response Body**

```shell
{
  "status":"OK"
  "message":" "
}
```


| Field | Description | Type |
| --- | --- | --- |
| status | Status Callback | String |
| message | Pesan tambahan oleh Merchant | String |




### 3.3 User Experience on Account Link

Pada section ini, akan dibahas apa saja yang akan dialami oleh Merchant User saat proses Account Link. Proses Account Link ini terdapat 4 bagian, yaitu:

1. Account Number Page
2. PIN Page
3. OTP Page
4. Account Link Result Page

Di bawah ini akan ditampilkan prototype untuk Account Link yang bisa dicoba oleh Merchant. Prototype ini akan menampilkan beberapa case, yaitu:

1. Account Link Success
2. Account Link Failed - Wrong PIN & Wrong OTP

### 3.3.1 Account Link Success



### 3.3.2 Account Link Failed - Wrong PIN & Wrong OTP



### 3.4 User Experience on Registration Page

Pada section ini, akan dibahas apa saja yang akan dialami oleh Merchant User saat proses Registration. Proses Registration ini terdapat 5 bagian, yaitu:

1. Account Number Page
2. Account Not Found Page
3. Registration Page
4. OTP Page
5. Registration Result Page

Di bawah ini akan ditampilkan prototype untuk Register yang bisa dicoba oleh Merchant. Prototype ini akan menampilkan beberapa case, yaitu:

1. Registration Success
2. Registration Failed - Wrong OTP

### 3.4.1 Registration Success



### 3.4.2 Registration Failed - Wrong OTP



Kami menyediakan 2 metode pembayaran pada Payment Channel AstraPay, yaitu:

1. Payment with linking 

2. Push to Payment

## 4. Payment with linking

Setelah melakukan proses Account Link, maka Merchant dapat mencoba pembayaran menggunakan account yang Merchant gunakan saat Account Link. Proses integrasi untuk Payment akan dijelaskan step by step di bawah ini.

### 4.1 Architecture

**Keterangan**

```shell
1. User melakukan checkout di Merchant
2. Merchant Backend melakukan hit request API ke AstraPay Backend untuk 
   mendapatkan token
3. AstraPay Backend memberikan response API berupa token dan url 
   Checkout Page ke Merchant Backend
4. Merchant Backend melakukan hit url ke Checkout Page AstraPay Webview
5. User melihat detail pembayaran dan klik tombol bayar
6. User memasukkan PIN dan AstraPay Backend melakukan validasi PIN
7. Setelah PIN terverifikasi, AstraPay Backend melakukan automatic redirect 
   ke webview sesuai default URL Merchant
8. Di waktu yang sama AstraPay Backend mengirimkan informasi status pembayaran 
   melalui payment callback ke Merchant Backend
```

Secara Arsitektur, metode pembayaran Merchant menggunakan AstraPay adalah sebagai berikut:
![img](./assets/Payment_Flow_Diagram-b60707cd_1.png)

### 4.2 Endpoint

**Complete Code**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/v1/merchant-service/payments' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--header 'Content-Type: application/json' \
--data-raw '{
    "signature": "b0950d6cdb9ba3f59a2baaa7e4a9cc6296b09d0a",
    "merchantTransactionId": "TRX000001",
    "amount": "10000",
    "currency": "IDR",
    "description": "buy account premium"
}'
```

Endpoint untuk metode Payment yang bisa Merchant hit adalah di bawah ini:

Protocol: HTTPS


Method: POST


URL Sandbox: https://sandbox.astrapay.com/v1/merchant-service/payments

Contoh complete request ada di sebelah kanan ini, detail-detail dari sample tersebut akan dijelaskan setelah poin ini.



### Request Header

**Sample Request Header**

```shell
{
    "Authorization": "Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q",
    "Content-Type": "application/json"
}
```

Request Header yang harus dimasukkan sebelum melakukan hit API Payment adalah sebagai berikut:

Authorization: Bearer token (Cara generate token ada pada section 2)


Content-Type: application/json



### Request Body

> [!WARNING]
> Sebelum melakukan request, pastikan Merchant memiliki akun yang sudah melakukan proses Account Link dan menggunakan clientId yang dipakai pada saat proses Account Link

Request Body yang dimasukkan oleh Merchant adalah sebagai berikut:

**Sample Request Body**

```shell
{ 
    "signature": "b0950d6cdb9ba3f59a2baaa7e4a9cc6296b09d0a",
    "merchantTransactionId": "TRX000001",
    "amount": "10000",
    "currency": "IDR",
    "description": "buy account premium"
}
```


| Field | Description | Type (Length) | Requirement |
| --- | --- | --- | --- |
| signature | String unik untuk menandakan integrasi antara akun AstraPay (account) dengan akun Merchant (merchantUserId) | String (255) | Mandatory |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat Merchant | String (255) | Mandatory |
| amount | Jumlah atau Nominal Transaksi. Min: 10000, Max: 10000000. | String (8) | Mandatory |
| currency | Jenis mata uang. Default: IDR (Selain IDR akan kami tolak) | String (3) | Mandatory |
| description | Catatan yang diperlukan | String (255) | Optional |


> [!WARNING]
> merchantTransactionId pada request ini adalah unik. Maka dari itu jika ada request menggunakan merchantTransactionId yang sama pada clientId yang sama, maka request akan ditolak.

### Response Body

**Sample Response Body**

```shell
{
    "token": "1c74a043-a28d-4826-a175-0bba189a83fd",
    "urlRedirect": "https://sandbox.astrapay.com/merchant-service/payments/1c74a043-a28d-4826-a175-0bba189a83fd"
}
```

Setelah Merchant berhasil melakukan request payment, maka Merchant akan mendapatkan respon sebagai berikut:


| Field | Description | Type |
| --- | --- | --- |
| token | Set of strings untuk memberikan akses Merchant User untuk melakukan transaksi menggunakan AstraPay.js (Coming Soon) | String |
| urlRedirect | URL Webview Payment AstraPay untuk Merchant User Melakukan Pembayaran | String |


urlRedirect inilah yang akan digunakan oleh Aplikasi Merchant untuk generate HTML AstraPay Payment Webview, adapun proses yang dialami oleh user akan dibahas pada 4.7.

Ketika proses Payment selesai, maka Merchant akan diredirect sesuai dengan Payment Default URL yang telah didaftarkan Merchant pada poin 1.2. dan akan seperti link seperti di samping.

**Sample URL**

```shell
https://www.merchant.com/redirect/payment/success?merchantTransactionId=TRX000001
```

**Sample App Deeplink**

```shell
merchantapp://app/payment/success?merchantTransactionId=TRX000001
```


| Field | Description | Type |
| --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat Merchant | String (255) |


Di saat yang bersamaan, Sistem AstraPay akan melakukan callback ke Payment Callback URL.



### 4.3 Payment Callback

> [!WARNING]
> Mohon pastikan untuk URL Callback yang didaftarkan oleh Merchant tidak memiliki Authorization

Setelah Merchant User selesai melakukan proses pembayaran dan sukses, maka AstraPay akan mengirimkan request kepada Payment Callback URL milik Merchant yang telah didaftarkan ke AstraPay pada poin 1.2. Request tersebut berfungsi sebagai notifikasi ke Merchant tentang status pembayaran dari Transaksi yang baru dilakukan.

Request yang dikirimkan oleh AstraPay akan berupa Request Body sebagai berikut:

Method: POST


Content-Type: application/json

**Sample Request Body**

```shell
{  
  "merchantTransactionId": "TRX000001"
  "astrapayTransactionId": "MP-001"
  "amount": "10000"
  "status": "Approved"
  "callbackTimestamp": "2021-08-31 08:25:48.363"
  "callbackSecurity": "c9ab3fa89b6c5b771e1162b50f1dd81f9f2c18b6015798c5ddcc5fb044a12519"
}
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat di Merchant | String (255) | Mandatory |
| astrapayTransactionId | Kode unik transaksi Merchant yang dicatat di AstraPay | String (255) | Mandatory |
| amount | Jumlah atau Nominal Transaksi. Min: 10000, Max: 10000000. | String (8) | Mandatory |
| status | Status Transaksi (Approved / Reject / Timeout ) | String (8) | Mandatory |
| callbackTimestamp | Waktu terjadinya pengiriman callback. (Generated Automatic by AstraPay) Format: yyyy-MM-dd HH:mm:ss.SSS | String | Mandatory |
| callbackSecurity | String hasil automatic generation oleh sistem astrapay dari Timestamp dan merchantTransactionId. | String (255) | Mandatory |


> [!WARNING]
> Cara melakukan Decrypt untuk callbackSecurity akan diberikan oleh AstraPay ke pada Merchant sesuai pada section 1.2. Selain itu AstraPay hanya melakukan Callback ketika status transaksi Approved,Reject, dan Timeout.

Lalu setelah itu Merchant akan mengirimkan response seperti ini:

**Sample Response Body**

```shell
{
  "status": "OK",
  "message": " "
}
```


| Field | Description | Type |
| --- | --- | --- |
| status | Status Callback | String |
| message | Pesan tambahan oleh Merchant | String |


Setelah proses Payment Notification selesai, maka proses pembayaran oleh Merchant User dan pencatatan oleh Merchant maupun AstraPay terhadap transaksi tersebut dapat dinyatakan selesai. Adapun proses-proses lain seperti refund, adalah proses yang berbeda dan dilakukan diluar proses ini.

### 4.4 User Experience on AstraPay Payment Webview

Pada section ini, akan dibahas apa saja yang akan dialami oleh Merchant User saat proses Payment. Proses Payment ini terdapat 3 bagian, yaitu:

1. Payment Detail Page
2. PIN Page
3. Payment Result Page

Di bawah ini akan ditampilkan prototype untuk Payment yang bisa dicoba oleh Merchant. Prototype ini akan menampilkan beberapa case, yaitu:

1. Payment Success
2. Payment Wrong PIN
3. Payment Expired

### 4.5 Payment Success



### 4.6 Payment Wrong PIN



### 4.7 Payment Expired



## 5. Push to Payment BETA

Push to payment adalah salah satu metode pembayaran di Payment Channel AstraPay, dengan metode ini user tidak perlu melakukan linking dengan Merchant. User hanya perlu menmasukkan nomor handphone yang sudah terdaftar pada AstraPay. Jika nomor yang dimasukkan belum terdaftar maka user akan diarahkan ke halaman registrasi seperti pada section **3.5 User Experience on Registration Page**

### 5.1 Architecture

**Keterangan**

```shell
1. User melakukan checkout di Merchant
2. Merchant Backend melakukan hit request API ke AstraPay Backend untuk 
   mendapatkan token
3. AstraPay Backend memberikan response API berupa token dan url 
   Checkout Page ke Merchant Backend
4. Merchant Backend melakukan hit url ke Checkout Page AstraPay Webview
5. User memasukkan nomor handphone yang terdaftar di AstraPay dan masuk 
   ke detail pembayaran
6. User melihat detail pembayaran dan klik tombol bayar
7. User memasukkan PIN dan AstraPay Backend melakukan validasi PIN
8. Setelah PIN terverifikasi, AstraPay Backend melakukan automatic redirect 
   ke webview sesuai default URL Merchant
9. Di waktu yang sama AstraPay Backend mengirimkan informasi status pembayaran 
   melalui payment callback ke Merchant Backend
```

Secara Arsitektur, alur metode pembayaran push to payment adalah sebagai berikut:
![img](./assets/Push_To_Payments-81a1a579_1.png)

### 5.2 Endpoint

**Complete Code**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/push-payments' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--header 'Content-Type: application/json' \
--data-raw '{
    "merchantTransactionId": "TRX000001",
    "amount": "10000",
    "currency": "IDR",
    "description": "buy using push to payment"
}'
```

Endpoint untuk metode push to payment yang bisa Merchant hit adalah di bawah ini:

Protocol: HTTPS


Method: POST


URL Sandbox: https://sandbox.astrapay.com/merchant-service/push-payments

Contoh complete request ada di sebelah kanan, detail dari sample tersebut akan dijelaskan setelah poin ini.



### Request Header

**Sample Request Header**

```shell
{
    "Authorization": "Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q",
    "Content-Type": "application/json"
}
```

Request Header yang harus dimasukkan sebelum melakukan hit API push to payment adalah sebagai berikut:

Authorization: Bearer token (Cara generate token ada pada section 2)


Content-Type: application/json



### Request Body

Request Body yang dimasukkan oleh Merchant adalah sebagai berikut:

**Sample Request Body**

```shell
{ 
    "merchantTransactionId": "TRX000001",
    "amount": "10000",
    "currency": "IDR",
    "description": "buy using push to payment"
}
```


| Field | Description | Type (Length) | Requirement |
| --- | --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat Merchant | String (255) | Mandatory |
| amount | Jumlah atau Nominal Transaksi. Min: 10000, Max: 10000000. | String (8) | Mandatory |
| currency | Jenis mata uang. Default: IDR (Selain IDR akan kami tolak) | String (3) | Mandatory |
| description | Catatan yang diperlukan | String (255) | Optional |


> [!WARNING]
> merchantTransactionId pada request ini adalah unik. Maka dari itu jika ada request menggunakan merchantTransactionId yang sama pada clientId yang sama, maka request akan ditolak.

### Response Body

Setelah Merchant berhasil melakukan request payment, maka Merchant akan mendapatkan respon sebagai berikut:

**Sample Response Body**

```shell
{
    "token": "1c74a043-a28d-4826-a175-0bba189a83fd",
    "urlRedirect": "https://sandbox.astrapay.com/merchant-service/payments/1c74a043-a28d-4826-a175-0bba189a83fd"
}
```


| Field | Description | Type |
| --- | --- | --- |
| token | Set of strings untuk memberikan akses Merchant User untuk melakukan transaksi menggunakan AstraPay.js (Coming Soon) | String |
| urlRedirect | URL Webview Payment AstraPay uxzntuk Merchant User Melakukan Pembayaran | String |


urlRedirect inilah yang akan digunakan oleh Aplikasi Merchant untuk generate HTML AstraPay Payment Webview, adapun proses yang dialami oleh user akan dibahas pada 4.7.

Ketika proses Payment selesai, maka Merchant akan diredirect sesuai dengan Payment Default URL yang telah didaftarkan Merchant pada poin 1.2. dan akan seperti link seperti di samping.

**Sample URL**

```shell
https://www.merchant.com/redirect/payment/success?merchantTransactionId=TRX000001
```

**Sample App Deeplink**

```shell
merchantapp://app/payment/success?merchantTransactionId=TRX000001
```


| Field | Description | Type |
| --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat Merchant | String (255) |


Di saat yang bersamaan, Sistem AstraPay akan melakukan callback ke Payment Callback URL.



### 5.3 Payment Callback

> [!WARNING]
> Mohon pastikan untuk URL Callback yang didaftarkan oleh Merchant tidak memiliki Authorization

Setelah Merchant User selesai melakukan proses pembayaran dan sukses, maka AstraPay akan mengirimkan request kepada Payment Callback URL milik Merchant yang telah didaftarkan ke AstraPay pada poin 1.2. Request tersebut berfungsi sebagai notifikasi ke Merchant tentang status pembayaran dari Transaksi yang baru dilakukan.

Request yang dikirimkan oleh AstraPay akan berupa Request Body sebagai berikut:

Method: POST


Content-Type: application/json

**Sample Request Body**

```shell
{  
  "merchantTransactionId": "TRX000001"
  "astrapayTransactionId": "MP-001"
  "amount": "10000"
  "status": "Approved"
  "callbackTimestamp": "2021-08-31 08:25:48.363"
  "callbackSecurity": "c9ab3fa89b6c5b771e1162b50f1dd81f9f2c18b6015798c5ddcc5fb044a12519"
}
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat di Merchant | String (255) | Mandatory |
| astrapayTransactionId | Kode unik transaksi Merchant yang dicatat di AstraPay | String (255) | Mandatory |
| amount | Jumlah atau Nominal Transaksi. Min: 10000, Max: 10000000. | String (8) | Mandatory |
| status | Status Transaksi (Approved / Reject / Timeout ) | String (8) | Mandatory |
| callbackTimestamp | Waktu terjadinya pengiriman callback. (Generated Automatic by AstraPay) Format: yyyy-MM-dd HH:mm:ss.SSS | String | Mandatory |
| callbackSecurity | String hasil automatic generation oleh sistem astrapay dari Timestamp dan merchantTransactionId. | String (255) | Mandatory |


> [!WARNING]
> Cara melakukan Decrypt untuk callbackSecurity akan diberikan oleh AstraPay ke pada Merchant sesuai pada section 1.2. Selain itu AstraPay hanya melakukan Callback ketika status transaksi Approved,Reject, dan Timeout.

Lalu setelah itu Merchant akan mengirimkan response seperti ini:

**Sample Response Body**

```shell
{
  "status": "OK",
  "message": " "
}
```


| Field | Description | Type |
| --- | --- | --- |
| status | Status Callback | String |
| message | Pesan tambahan oleh Merchant | String |


Setelah proses Payment Notification selesai, maka proses pembayaran oleh Merchant User dan pencatatan oleh Merchant maupun AstraPay terhadap transaksi tersebut dapat dinyatakan selesai. Adapun proses-proses lain seperti refund, adalah proses yang berbeda dan dilakukan diluar proses ini.

## 6. Profile

Pada section ini akan membahas API Profile. API ini berguna untuk mendapatkan informasi user dengan menggunakan signature yang telah diberikan pada section 3.

### 6.1 Endpoint

**Complete Code**

```shell
curl --location --request GET 'https://sandbox.astrapay.com/merchant-service/profile?signature=b0950d6cdb9ba3f59a2baaa7e4a9cc6296b09d0a' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
```

Endpoint untuk proses Profile yang bisa Merchant hit adalah di bawah ini:

Protocol: HTTPS


Method: GET


URL Sandbox: https://sandbox.astrapay.com/merchant-service/profile



### Request Header

**Sample Request Header**

```shell
{
    "Authorization": "Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q",
}
```

Request Header yang harus dimasukkan sebelum melakukan hit API Profile adalah sebagai berikut:

Authorization: Bearer token (Cara generate token ada pada section 2)



### Request Parameter

Request Parameter yang harus dimasukkan sebelum melakukan hit endpoint Profile adalah sebagai berikut.

**Sample Request Parameter**

```shell
https://sandbox.astrapay.com/merchant-service/profile?signature=b0950d6cdb9ba3f59a2baaa7e4a9cc6296b09d0a
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| signature | String unik yang menandakan bahwa Account AstraPay sudah terintegrasi dengan aplikasi Merchant | String (255) | Mandatory |


### Response Body

Setelah melakukan request pada endpoint Profile dan telah tervalidasi, maka Merchant akan mendapatkan response sebagai berikut:

**Sample Response Body**

```shell
{
    "balance": "5822.0"
}
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| balance | Saldo akun AstraPay. User Classic Max: 2000000, User Preferred Max: 10000000 | String | Mandatory |


## 7. Transaction Status

Pada section ini akan membahas API Transaction Status. API ini berguna untuk mendapatkan informasi transaksi merchant dengan menggunakan clientId dan merchantTransactionId yang telah dikirimkan pada section 4.

### 7.1 Endpoint

**Complete Code**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/transaction-status' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--data-raw '{
    "merchantTransactionId": "TRX000001"
}'
```

Endpoint untuk proses Transaction Status yang bisa Merchant hit adalah di bawah ini:

Protocol: HTTPS


Method: POST


URL Sandbox: https://sandbox.astrapay.com/merchant-service/transaction-status



### 7.2 Request Header

**Sample Request Header**

```shell
{
  Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q,
  Content-Type: application/json
}
```

Request Header yang harus dimasukkan sebelum melakukan hit endpoint Transaction Status adalah sebagai berikut:

Authorization: Bearer token (Cara generate token ada pada section 2)


Content-Type: appplication/json



### Request Body

Request Body yang harus dimasukkan adalah sebagai berikut.

**Sample Request Body**

```shell
{
    "merchantTransactionId": "TRX000001"
}
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat di Merchant | String (255) | Mandatory |




### Response Body

Setelah melakukan request pada API Transaction Status dan telah tervalidasi, maka Merchant akan mendapatkan response sebagai berikut:

**Sample Response Body**

```shell
{
  "merchantTransactionId": "TRX000001"
  "astrapayTransactionId": "MP-001"
  "amount": "10000"
  "status": "APP"
}
```


| Field | Description | Type |
| --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat di Merchant | String (255) |
| astrapayTransactionId | Kode unik transaksi Merchant yang dicatat di AstraPay | String (255) |
| amount | Jumlah atau Nominal Transaksi. Min: 1, Max: 10000000 | String |
| status | Status Transaksi (APP, REJ, PND, TIM) | String (3) |


APP = "Approved", Transaksi Sukses di AstraPay


REJ = "Rejected", Transaksi Gagal di AstraPay


PND = "Pending", Transaksi sedang dalam proses pembayaran.


TIM = "Timeout", Transaksi Gagal diproses di AstraPay karena Timeout

## 8. Refund BETA

Pada section ini akan membahas API Refund. API ini berfungsi untuk membuat request refund (pengembalian dana) saldo pengguna yang telah bertransaksi di merchant.

AstraPay dapat memberikan pengembalian dana untuk transaksi yang pengguna lakukan di merchant atas permintaan dari merchant. Saat ini, proses refund baru dapat dilakukan  H+1 setelah Transaksi dilakukan (Setelah Settlement).

### 8.1 Endpoint

**Complete Code**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/merchant-service/v1.0/customer-refund' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU95STd1R0g0dV9ZMzBmeTc4b1I1T2V6S3JIdVpzIn0.eyJleHAiOjE2NTk2MDM1ODMsImlhdCI6MTY1OTU5OTk4MywianRpIjoiNzdjMTE3NzYtMzY5YS00MGYxLTllZGItMmM3MjNjNzJiMTI0IiwiaXNzIjoiaHR0cDovLzEwLjIwLjYuOTo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjQ2NDg1MTRhLTY3YjMtNGRhZi04YjYzLTJiNWIxZmNjNGMwYiIsInR5cCI6IkJlYXJlciIsImF6cCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJjbGllbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImNsaWVudEhvc3QiOiIxNzIuMjAuNS4yMSIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LWMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNS4yMSJ9.zNGxL4I5vNrOMqDflyfsN0jGGj-VDHz-37OkM4qSWKgN6gDIq_pMexcXaukdQSE2iT82GQPdB6euVHoxozrse8ck8WCepEpL5yhopyfm9llqKbT1WsrRw4nYp9kBJssGSXJDvqbp1nPffj-88K_GcLVReRLs9pMml-1ckkf72zAR1vX6VKFaP-ULMsYvubs1pGC70PYY3bWjFoe4nXQ70BOwFGbrRzKrX8_nfAgoaIJCF1eqMw2PJmIY2Pl25a8djMEDLcqKSJpUjmjH2gmSTS_til0zRUkt_E321YPASR4YkcycO067pBPBJ7nlaUdaP5Vkv_c4L6WZJKYjZrNoDQ' \
--header 'X-ASTRAPAY-KEY: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU95STd1R0g0dV9ZMzBmeTc4b1I1T2V6S3JIdVpzIn0.eyJleHAiOjE2NTk2MDM1ODMsImlhdCI6MTY1OTU5OTk4MywianRpIjoiNzdjMTE3NzYtMzY5YS00MGYxLTllZGItMmM3MjNjNzJiMTI0IiwiaXNzIjoiaHR0cDovLzEwLjIwLjYuOTo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjQ2NDg1MTRhLTY3YjMtNGRhZi04YjYzLTJiNWIxZmNjNGMwYiIsInR5cCI6IkJlYXJlciIsImF6cCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJjbGllbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImNsaWVudEhvc3QiOiIxNzIuMjAuNS4yMSIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LWMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNS4yMSJ9.zNGxL4I5vNrOMqDflyfsN0jGGj-VDHz-37OkM4qSWKgN6gDIq_pMexcXaukdQSE2iT82GQPdB6euVHoxozrse8ck8WCepEpL5yhopyfm9llqKbT1WsrRw4nYp9kBJssGSXJDvqbp1nPffj-88K_GcLVReRLs9pMml-1ckkf72zAR1vX6VKFaP-ULMsYvubs1pGC70PYY3bWjFoe4nXQ70BOwFGbrRzKrX8_nfAgoaIJCF1eqMw2PJmIY2Pl25a8djMEDLcqKSJpUjmjH2gmSTS_til0zRUkt_E321YPASR4YkcycO067pBPBJ7nlaUdaP5Vkv_c4L6WZJKYjZrNoDQ' \
--header 'X-SIGNATURE: 835663a9bb06955a905cc8d90262322dcac4bd25621b18b5fe6a86a4b3df287a' \
--header 'X-TIMESTAMP: 2022-04-21T11:37:51.436Z' \
--data-raw '{
    "originalPartnerReferenceNo" : "TRANSACTION-TEST-08",
        "partnerRefundNo": "PARTNER/NO/123/008",
        "refundAmount": {
            "amount": 10000,
            "currency": "IDR"
        },
    "reason": "Komplain saldo terpotong tetapi pembayaran gagal",
    "refundType": "FULL"
}'
```

Endpoint untuk proses Refund yang bisa Merchant hit adalah di bawah ini:

Protocol: HTTPS


Method: POST


URL Sandbox: https://sandbox.astrapay.com/merchant-service/v1.0/customer-refund



### Request Header

**Sample Request Header**

```shell
{
  Content-Type: application/json
  Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU95STd1R0g0dV9ZMzBmeTc4b1I1T2V6S3JIdVpzIn0.eyJleHAiOjE2NTEzMDgzNDMsImlhdCI6MTY1MTMwNDc0MywianRpIjoiZTc3MDE4MmQtNmMyZi00MjgyLWFlMzMtZWViNGRhY2I2ZDdiIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6ImYxOTI0NjhkLWZhYzEtNDZkMy05NWQ0LTRkODFiOTBkYmEwNCIsInR5cCI6IkJlYXJlciIsImF6cCI6Ijg2MzQ0YTJiLWMwY2YtNGQzMi04ZDVkLWJmY2ViNDQ0MGYwMiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6Ijg2MzQ0YTJiLWMwY2YtNGQzMi04ZDVkLWJmY2ViNDQ0MGYwMiIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC04NjM0NGEyYi1jMGNmLTRkMzItOGQ1ZC1iZmNlYjQ0NDBmMDIiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.omdTLU4guhdpN30BmtUw2dvxEHIIRnceFph6csUj-Hd3SNH1fjkb5h0dvK0vyV5lLuYChBcuztr3LbIvydsE_ZNAwGd0YsyioBfuuOUROWXYodsZtqV-3KqT-o3G6c8ySSxn0-Twy0ETVwlGA1TClJDRIAXu4sQUD4xAYyRF4PYkLsOUf30I6698d_rJo8cjZtjdg5lqzTMMJB8juKDR7uikMIFJqUGSZh7wnOM3DnF1yTN9d2whOq533D9xDl3EOTQ__n50KOyrB32muxoXuN-FPcoHuOvY62blofmaECrSRl8PGEmuW8w0keM3vWNorme2NOuX8tpM3qMko4adZg
  X-ASTRAPAY-KEY: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJUZk50bGRXTzhvRGhibU95STd1R0g0dV9ZMzBmeTc4b1I1T2V6S3JIdVpzIn0.eyJleHAiOjE2NTk2MDM1ODMsImlhdCI6MTY1OTU5OTk4MywianRpIjoiNzdjMTE3NzYtMzY5YS00MGYxLTllZGItMmM3MjNjNzJiMTI0IiwiaXNzIjoiaHR0cDovLzEwLjIwLjYuOTo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjQ2NDg1MTRhLTY3YjMtNGRhZi04YjYzLTJiNWIxZmNjNGMwYiIsInR5cCI6IkJlYXJlciIsImF6cCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJjbGllbnRJZCI6ImMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImNsaWVudEhvc3QiOiIxNzIuMjAuNS4yMSIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LWMxOGVjMzVmLTU3ZDYtNGY2OS1iOTdlLWIyZWYyY2U3OGFiYiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNS4yMSJ9.zNGxL4I5vNrOMqDflyfsN0jGGj-VDHz-37OkM4qSWKgN6gDIq_pMexcXaukdQSE2iT82GQPdB6euVHoxozrse8ck8WCepEpL5yhopyfm9llqKbT1WsrRw4nYp9kBJssGSXJDvqbp1nPffj-88K_GcLVReRLs9pMml-1ckkf72zAR1vX6VKFaP-ULMsYvubs1pGC70PYY3bWjFoe4nXQ70BOwFGbrRzKrX8_nfAgoaIJCF1eqMw2PJmIY2Pl25a8djMEDLcqKSJpUjmjH2gmSTS_til0zRUkt_E321YPASR4YkcycO067pBPBJ7nlaUdaP5Vkv_c4L6WZJKYjZrNoDQ
  X-SIGNATURE: 835663a9bb06955a905cc8d90262322dcac4bd25621b18b5fe6a86a4b3df287a
  X-TIMESTAMP: 2022-04-21T11:37:51.436Z
}
```

Request Header yang harus dimasukkan sebelum melakukan hit endpoint Refund adalah sebagai berikut:




| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| Content-Type | Application/json | String | Mandatory |
| Authorization | Bearer token (Cara generate token ada pada section 2) | String | Mandatory |
| X-ASTRAPAY-KEY | token dari Authorization | String | Mandatory |
| X-SIGNATURE | Cara generate X-Signature ada pada menu Disbursement, BAB Signature | String | Mandatory |
| X-TIMESTAMP | Waktu lokal client saat ini dengan format yyyy-MM-dd'T'HH:mm:ss.SSS'Z' | String | Mandatory |


### Request Body

Request Body yang harus dimasukkan adalah sebagai berikut.

**Sample Request Body**

```shell
{
    "originalPartnerReferenceNo" : "TRANSACTION-TEST-08",
        "partnerRefundNo": "PARTNER/NO/123/008",
        "refundAmount": {
            "amount": 10000,
            "currency": "IDR"
        },
    "reason": "Komplain saldo terpotong tetapi pembayaran gagal",
    "refundType": "FULL"
}
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| originalPartnerReferenceNo | merchantTransactionId dari payment sebelumnya | String (255) | Mandatory |
| partnerRefundNo | Kode unik transaksi dari Merchant yang akan dicatat ke AstraPay | String (255) | Mandatory |
| refundAmount | Terdiri dari nilai total (amount) transaksi dan mata uang (currency) yang akan di kembalikan | Object | Mandatory |
| amount | Nilai total transaksi | String sesuai (ISO4217) | Mandatory |
| currency | Mata uang (IDR) | String (3) | Mandatory |
| reason | Alasan pengembalian dilakukan | String (255) | Mandatory |
| refundType | tipe refund diisi FULL atau PARTIAL(Coming Soon) | String (255) | Mandatory |




### Response Header

Response Header yang akan diterima adalah sebagai berikut

**Sample Response Header**

```shell
{
  HTTP: 202
  Content-type: application/json
}
```


| Field | Description | Type | Requirement |
| --- | --- | --- | --- |
| HTTP | Kode HTTP bisa 202 atau 400 | String | Mandatory |
| Content-type | application/json | String | Mandatory |


### Response Body

Setelah melakukan request pada API Refund dan telah tervalidasi, maka Merchant akan mendapatkan response sebagai berikut:

**Sample Response Body**

```shell
{
   "responseCode":"2020300",
   "responseMessage":"Request In Progress",
   "originalReferenceNo":"082230872500.MP.20220421", 
   "originalPartnerReferenceNo": "TRANSACTION-TEST-08", 
   "refundNo":"0fc68e5c-233b-4cda-ac83-ffc6717cf5c5", 
   "partnerRefundNo":"PARTNER/NO/123/008", 
   "refundAmount":{
      "amount":"10000",
      "currency":"IDR"
   },
   "reason":"Komplain saldo terpotong tetapi pembayaran gagal" 
}
```


| Field | Description | Type |
| --- | --- | --- |
| responseCode | kode response astrapay terdiri dari HTTP Status + Kode dari Astrapay | String (7) |
| responseMessage | Deskripsi respon dari AstraPay | String (255) |
| originalReferenceNo | Kode astrapayTransactionNumber dari payment | String |
| refundNo | Kode unik pengajuan pengembalian yang digenerate oleh AstraPay | String |
| partnerRefundNo | Kode unik transaksi dari Merchant yang akan dicatat AstraPay | String |
| refundAmount | Terdiri dari nilai total transaksi (amount) dan mata uang (IDR) (currency) yang akan di kembalikan | Object |
| amount | Nilai total transaksi | String sesuai (ISO4217) |
| currency | Mata uang (IDR) | String |
| reason | Alasan refund diajukan | String |


## 9. Cancel BETA

Pada section ini akan membahas API Cancel. API ini berfungsi untuk membatalkan transaksi yang sedang berlangsung.
Saat ini, proses cancel dapat dilakukan sebelum transaksi yang dilakukan selesai atau ketika masih dalam status PND.

### 9.1 Endpoint

**Complete Code**

```shell
curl --location --request PATCH 'https://sandbox.astrapay.com/merchant-service/payments/TRANSACTION-TEST-08/cancel' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE2NjI1MzU5MTUsImlhdCI6MTY2MjUzMjMxNSwianRpIjoiMWRhODU3MDctZWMxZi00NWNiLWE5OWQtN2I1N2M2YTM4OTcxIiwiaXNzIjoiaHR0cDovLzEwLjIwLjcuNjo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjNkMDUyMzg3LWZiYTItNDIyMy04M2EyLWM1ZmE4N2U4N2Y1MSIsInR5cCI6IkJlYXJlciIsImF6cCI6ImY5MWZhYmZkLTMyMzEtNDU4ZS04MzIwLWFlOTliN2RjYWJkMiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJjbGllbnRJZCI6ImY5MWZhYmZkLTMyMzEtNDU4ZS04MzIwLWFlOTliN2RjYWJkMiIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiY2xpZW50SG9zdCI6IjEwMy4xNTQuOTMuMTU0IiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LWY5MWZhYmZkLTMyMzEtNDU4ZS04MzIwLWFlOTliN2RjYWJkMiIsImNsaWVudEFkZHJlc3MiOiIxMDMuMTU0LjkzLjE1NCJ9.DAKf1ZJkwrfdWEJObEuTClCIp5BByKRBZ6ipWYGRb_cBl_NDkFO5-dK5eZymLAvHajGfLF5DWPxjRrrdtsSPSa4v328p44WaDk8TW99gtgqN8ScZmMvnYHn-UtaezvAKJrxoJyie_kjqmty7SilSPq31x8eHJK0Lznv5DsDzEIYkNrlbj8ob7J8PYPAtN529VASubVAvvrv2-bgayMpYeMI_Vt7DOxZTzUCM4CsZDUAd51csAoYB0m2qiqXupPm8z9dOQy5iNTcuRiKPKdM5r9DZpkTspS0HQZ3Z3C9NGZTtercAEJ-dZq1_qZAhOefOav8bfspqGNFOZYVKbzCHKA' \
--data-raw ''
```

Endpoint untuk proses Cancel yang bisa Merchant hit adalah di bawah ini:

Protocol: HTTPS


Method: POST


URL Sandbox: https://sandbox.astrapay.com/merchant-service/payments/{merchantTransactionId}/cancel



### 9.2 Request Header

**Sample Request Header**

```shell
{
  Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE2NjI1MzU5MTUsImlhdCI6MTY2MjUzMjMxNSwianRpIjoiMWRhODU3MDctZWMxZi00NWNiLWE5OWQtN2I1N2M2YTM4OTcxIiwiaXNzIjoiaHR0cDovLzEwLjIwLjcuNjo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjNkMDUyMzg3LWZiYTItNDIyMy04M2EyLWM1ZmE4N2U4N2Y1MSIsInR5cCI6IkJlYXJlciIsImF6cCI6ImY5MWZhYmZkLTMyMzEtNDU4ZS04MzIwLWFlOTliN2RjYWJkMiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJjbGllbnRJZCI6ImY5MWZhYmZkLTMyMzEtNDU4ZS04MzIwLWFlOTliN2RjYWJkMiIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiY2xpZW50SG9zdCI6IjEwMy4xNTQuOTMuMTU0IiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LWY5MWZhYmZkLTMyMzEtNDU4ZS04MzIwLWFlOTliN2RjYWJkMiIsImNsaWVudEFkZHJlc3MiOiIxMDMuMTU0LjkzLjE1NCJ9.DAKf1ZJkwrfdWEJObEuTClCIp5BByKRBZ6ipWYGRb_cBl_NDkFO5-dK5eZymLAvHajGfLF5DWPxjRrrdtsSPSa4v328p44WaDk8TW99gtgqN8ScZmMvnYHn-UtaezvAKJrxoJyie_kjqmty7SilSPq31x8eHJK0Lznv5DsDzEIYkNrlbj8ob7J8PYPAtN529VASubVAvvrv2-bgayMpYeMI_Vt7DOxZTzUCM4CsZDUAd51csAoYB0m2qiqXupPm8z9dOQy5iNTcuRiKPKdM5r9DZpkTspS0HQZ3Z3C9NGZTtercAEJ-dZq1_qZAhOefOav8bfspqGNFOZYVKbzCHKA,
  Content-Type: application/json
}
```

Request Header yang harus dimasukkan sebelum melakukan hit endpoint Cancel adalah sebagai berikut:

Authorization: Bearer token (Cara generate token ada pada section 2)


Content-Type: application/json



### Response Body

Setelah melakukan request pada API Cancel dan telah tervalidasi, maka Merchant akan mendapatkan response sebagai berikut:

**Sample Response Body**

```shell
{
    "merchantTransactionId": "Paket Hemat 001",
    "message": "Transaction canceled"
}
```


| Field | Description | Type |
| --- | --- | --- |
| merchantTransactionId | Kode unik transaksi Merchant yang dicatat di Merchant | String |
| message | Detail pesan response yang diberikan | String |


## 10. Response List

Astrapay menggunakan konvensional HTTP Response untuk menunjukkan keberhasilan atau kegagalan dalam permintaan API. Berikut ini adalah beberapa error paling umum dalam endpoint kami:


| Status Code | Description |
| --- | --- |
| 200 | Permintaan API berhasil dan bekerja sebagaimana mestinya |
| 400 | Permintaan tidak diterima karena ada kesalahan pada parameter |
| 401 | Akses tidak terotorisasi, contoh: API key tidak valid |
| 403 | Akses terlarang, contoh: API tidak memiliki akses |
| 500 | Error tidak diketahui atau ada masalah pada Astrapay End. Hubungi kami jika terjadi. |


### 10.1 Payment


| Status Code | Message | Description |
| --- | --- | --- |
| 400 | Your Balance Is Not Enough | Saldo AstraPay tidak cukup untuk melakukan transaksi |
|  | Currency must using IDR | Mata uang harus menggunakan IDR/Rupiah |
|  | Character only allow alphanumeric, dot, dash, tilda, underscore, slash or backslash | Karakter yang boleh digunakan hanya alfanumerik, titik, tilda, garis bawah, dan garis miring |
|  | Must be greater than or equal to 1 | Harga harus lebih dari atau sama dengan Rp1 |
|  | Size limit merchantTransactionId is 255 Characters | Karakter pada merchantTransactionID tidak boleh lebih dari 255 karakter |
|  | Signature not found | Signature pengguna AstraPay tidak ditemukan |
|  | Transaction ID already exist | ID Transaksi sudah digunakan |


### 10.2 Profile


| Status Code | Message | Description |
| --- | --- | --- |
| 400 | Signature does not belong to this merchant | Signature pengguna ditemukan, tapi tidak terdaftar pada merchant |
|  | Signature not found | Signature pengguna AstraPay tidak ditemukan |


### 10.3 Transaction Status


| Status Code | Message | Description |
| --- | --- | --- |
| 400 | Merchant transaction ID not found | Merchant transaction ID tidak ditemukan |
|  | Size limit merchantTransactionId is 255 Characters | Karakter pada merchantTransactionID tidak boleh lebih dari 255 karakter |


### 10.4 Refund


| Status Code | Message | Description |
| --- | --- | --- |
| 202 | Request In Progress | Permintaan sedang di proses |
| 400 | Invalid Amount | Nominal Salah |
|  | Your refund request not refundable | request refund tidak bisa dilakukan |
|  | Your refund number already exist | nomor refund sudah ada |
|  | Your refund request not allowed | request refund tidak diizinkan |
|  | Transactions more than a month cannot be refunded | transaksi lebih dari sebulan tidak bisa di refund |


### 10.4 Cancel


| Status Code | Message | Description |
| --- | --- | --- |
| 200 | Transaction canceled | Transaksi berhasil dibatalkan |
| 400 | Merchant Transaction ID is Not Eligible | Merchant Transaction ID Tersebut sudah tidak bisa di cancel |


## 11. User Acceptance Testing

Jika proses development telah selesai, setiap merchant diwajibkan untuk mengikuti pelaksanaan User Acceptance Testing (UAT). 
Untuk pelaksanaan UAT tersebut dilaksanakan oleh pihak Merchant. Untuk skenario testing yang akan dilakukan bisa langsung request form uat ke AstraPay.
