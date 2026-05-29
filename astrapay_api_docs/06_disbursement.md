# Disbursement


## Alur Disbursement

![img](./assets/flow-hosttohost-disbursement-d5fd8e15.png)

## Alur Skenario Timeout Disbursement

Pada Disbursement Service memiliki Service Level Agreement (SLA) untuk response dari API Disbursement, yaitu selama **45 detik**, maka merchant diharuskan untuk advice terhadap proses disbursement yang dilakukan. Berikut gambar alur untuk skenario timeout.

![img](./assets/flow-timeout-disbursement-c92f5b29.png)

Untuk melakukan disbursement saldo AstraPay, sistem kami akan memvalidasi tipe dari setiap akun tujuan. Harap memperhatikan limit dari setiap tipe akun yang menjadi tujuan transaksi, pastikan tipe user yaitu Classic dan Preferred sudah sesuai dengan kriteria dibawah :

- Tipe akun `Classic` : 


    - Memiliki maximum saldo `Rp.2.000.000`
    - Dan memiliki maximum transaksi bulanan sebesar `Rp.20.000.000`
- Tipe akun `Preferred` :


    - Memiliki maximum saldo `Rp.10.000.000`
    - Dan memiliki maximum transaksi bulanan sebesar `Rp.20.000.000`

## Pembuatan Token Akses

Authorization merupakan sebuah string token yang akan dipakai sebagai Request Header pada proses integrasi nanti, namun untuk mendapatkan Authorization ini Merchant Disbursement perlu melakukan proses generate `access_token`.

Endpoint untuk proses Generate Token memiliki URL seperti di bawah ini:

**Protocol**: HTTPS

**Method**: POST

**URL Sandbox**: https://sandbox.astrapay.com/api/oauth/token

**Sample Request Header**

```shell
Content-Type: application/x-www-form-urlencoded
```

**Sample Request Body**

```shell
grant_type: client_credentials
  client_id: a9652973-4a43-434f-924f-f584f50feed6
  client_secret: ck2IwGsOvZa8L15kCTjCB2bvGYsLFDv4
```

### Request


| Field | Tipe Data | Wajib | Deskripsi |
| --- | --- | --- | --- |
| grant_type | String | Y | Validasi jenis akses. Format: client_credentials |
| client_id | String(36) | Y | String unik sebagai ID untuk menandakan Merchant yang mengirim request |
| client_secret | String(32) | Y | String unik rahasia untuk menandakan Merchant yang mengirim request |


**Sample Response Body**

```shell
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q",
    "expires_in": 3600,
    "refresh_expires_in": 0,
    "token_type": "Bearer",
    "not-before-policy": 0,
    "scope": "profile email"
}
```

### Kode Error


| Status Code | Message | Deskripsi |
| --- | --- | --- |
| 200   **OK** |  | Client berhasil teridentifikasi dan akses token diberikan. |
| 400   **Bad Request** | Invalid client credentials | Otentikasi klien gagal `client_id` atau `client_secret` tidak dikenal. |
|  | Unsupported grant_type | value `grant_type` yang tidak sesuai. |
|  | Client secret not provided in request | `client_secret` tidak disertakan dalam request. |
|  | Missing form parameter: grant_type | value `grant_type` tidak disertakan dalam request. |
| 500   **Internal Server Error** |  | Respons tidak valid dari Back-end **atau** Kesalahan Tidak Terdefinisi |


## Signature

Signature digunakan oleh AstraPay untuk memverifikasi bahwa request dari Host tidak tersusupi oleh hacker.

### Generate Signature

**Contoh StringToSign**

```shell
HttpMethod   : POST
Url          : /disbursement-service/h2h/inquiries
AstraPayKey  : eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJiOThDaFhUWFVJQ3J4SjhadWt5OWNCUXpBLThqQnhxa0lndmpoM0h3SVpFIn0.eyJleHAiOjE2NDg2MjE2ODksImlhdCI6MTY0ODYyMTM4OSwianRpIjoiM2RlZmY3MzAtZTllZi00MWM5LTk4MWYtZDdlOTcyYjZiNjMzIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC42Ljc5OjgwODAvYXV0aC9yZWFsbXMvYXN0cmFwYXktYnVzaW5lc3MiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMmQzMGUzYjAtNjk0Yi00NjkyLWE4YmUtYmJhNTA5ZjBjNjQ5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiY2xpZW50SG9zdCI6IjE3Mi4yMC42LjEwOSIsImNsaWVudElkIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LTgxOTY5NjEyLWFmZjEtMTFlYy1iOTA5LTAyNDJhYzEyMDAwMiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNi4xMDkifQ.dcVfUrHe53BXZU2abOvSxFkZy59BcEO6Zb3jLMlU9FL_XBxN09lPSgQ_MY2ejksz0oTHkyjlRz9apG6c3n5GY34hj6jjY_UFV6A97VVv53F0nLqdpTlFKQYuzIrZs4BJmUzvBGviTI9K9dNagQ6rop6LwsqzJxGT8laS5EtIDDqaP4kciADqv0xOaWZfqYE9nbz91w54qQ5vT2y64XeINoQXFzr7hvhmXY8sgbCWDgKdlxnWU_oN-76EJmT2Yfa66aEMF8vIL7NLk_6_MsVduIdvge5covOltsWnICOZUHThfDj-CLbzUL55d62h7PkMmOKlCdl9HCS8pa_CKK4Rtg
RequestBody  : {"userIdentification":"085939526552","amount":15000}
Timestamp    : 2021-04-21T11:37:51.436Z

StringToSign : POST:/disbursement-service/h2h inquiries:eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJiOThDaFhUWFVJQ3J4SjhadWt5OWNCUXpBLThqQnhxa0lndmpoM0h3SVpFIn0.eyJleHAiOjE2NDg2MjE2ODksImlhdCI6MTY0ODYyMTM4OSwianRpIjoiM2RlZmY3MzAtZTllZi00MWM5LTk4MWYtZDdlOTcyYjZiNjMzIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC42Ljc5OjgwODAvYXV0aC9yZWFsbXMvYXN0cmFwYXktYnVzaW5lc3MiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMmQzMGUzYjAtNjk0Yi00NjkyLWE4YmUtYmJhNTA5ZjBjNjQ5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiY2xpZW50SG9zdCI6IjE3Mi4yMC42LjEwOSIsImNsaWVudElkIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LTgxOTY5NjEyLWFmZjEtMTFlYy1iOTA5LTAyNDJhYzEyMDAwMiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNi4xMDkifQ.dcVfUrHe53BXZU2abOvSxFkZy59BcEO6Zb3jLMlU9FL_XBxN09lPSgQ_MY2ejksz0oTHkyjlRz9apG6c3n5GY34hj6jjY_UFV6A97VVv53F0nLqdpTlFKQYuzIrZs4BJmUzvBGviTI9K9dNagQ6rop6LwsqzJxGT8laS5EtIDDqaP4kciADqv0xOaWZfqYE9nbz91w54qQ5vT2y64XeINoQXFzr7hvhmXY8sgbCWDgKdlxnWU_oN-76EJmT2Yfa66aEMF8vIL7NLk_6_MsVduIdvge5covOltsWnICOZUHThfDj-CLbzUL55d62h7PkMmOKlCdl9HCS8pa_CKK4Rtg:77bf1abdccbeeff284c4f06b99d5cf9e22bfa56cc744aa5784da873275a1c86c:2021-04-21T11:37:51.436Z
```

> [!NOTE]
> StringToSign = HTTPMethod + ":" + Url + ":" + AstraPayKey + ":" + HexEncode(SHA-256(RequestBody)) + ":" + Timestamp


| Value | Deskripsi | Contoh |
| --- | --- | --- |
| HTTPMethod | Jenis dari HTTP Method, tetap dalam value **UPPERCASE** | POST, GET |
| Url | Keseluruhan Uniform Resource Locator yang diakses oleh Host, semua karakter setelah nama domain | /disbursement-service/h2h/inquiries |
| AstraPayKey | [Bearer Token](#pembuatan-token-akses) yang dikirim melalui request header dengan field `x-astrapay-key` | eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJiOThDaFhUWFVJQ3J4SjhadWt5OWNCUXpBLThqQnhxa0lndmpoM0h3SVpFIn0.eyJleHAiOjE2NDg2MjE2ODksImlhdCI6MTY0ODYyMTM4OSwianRpIjoiM2RlZmY3MzAtZTllZi00MWM5LTk4MWYtZDdlOTcyYjZiNjMzIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC42Ljc5OjgwODAvYXV0aC9yZWFsbXMvYXN0cmFwYXktYnVzaW5lc3MiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMmQzMGUzYjAtNjk0Yi00NjkyLWE4YmUtYmJhNTA5ZjBjNjQ5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiY2xpZW50SG9zdCI6IjE3Mi4yMC42LjEwOSIsImNsaWVudElkIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LTgxOTY5NjEyLWFmZjEtMTFlYy1iOTA5LTAyNDJhYzEyMDAwMiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNi4xMDkifQ.dcVfUrHe53BXZU2abOvSxFkZy59BcEO6Zb3jLMlU9FL_XBxN09lPSgQ_MY2ejksz0oTHkyjlRz9apG6c3n5GY34hj6jjY_UFV6A97VVv53F0nLqdpTlFKQYuzIrZs4BJmUzvBGviTI9K9dNagQ6rop6LwsqzJxGT8laS5EtIDDqaP4kciADqv0xOaWZfqYE9nbz91w54qQ5vT2y64XeINoQXFzr7hvhmXY8sgbCWDgKdlxnWU_oN-76EJmT2Yfa66aEMF8vIL7NLk_6_MsVduIdvge5covOltsWnICOZUHThfDj-CLbzUL55d62h7PkMmOKlCdl9HCS8pa_CKK4Rtg |
| RequestBody | Isi dari Body pada request, dalam bentuk **compact** / **minify** (menghapus semua tabs, whitespace) sebelum dilakukan hash dengan algoritma SHA-256 | {"userIdentification":"08123456789","amount":15000} |
| Timestamp | Timestamp dengan format **ISO8601** dan dalam format waktu **UTC**, `yyyy-MM-ddTHH:mm:ss.SSSZ`. Dan dikirim melalui request header dengan field `x-timestamp` | 2021-04-21T11:37:51.436Z |


**Contoh Request Header**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/disbursement-service/h2h/inquiries' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--header 'x-signature: fb0a2ae94f0148452459dd1f070d00194c607bb5636e149e5fc271d0a7da825b' \
--header 'x-timestamp: 2021-04-21T11:37:51.436Z' \
--header 'x-astrapay-key: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--header 'Content-Type: application/json'
```

> [!NOTE]
> AstraPaySignature = HMAC-SHA256([StringToSign](#stringtosign), AstraPayValidationKey)


| Value | Deskripsi | Contoh |
| --- | --- | --- |
| AstraPayValidationKey | ValidationKey yang dikirimkan kepada Host dan bersifat rahasia | String dengan panjang 44 karakter |
| AstraPaySignature | Hasil generate signature dengan `SHA-256 HMAC` dengan komposisi dari StringToSign dan AstraPayValidationKey, dan dikirim melalui request header dengan field `x-signature` | fb0a2ae94f0148452459dd1f070d00194c607bb5636e149e5fc271d0a7da825b |


**Respon Error Signature Tidak Sesuai
400 - Bad Request**

```text
Invalid signature
```

## Inquiry Disbursement

*Tahap ini merupakan proses awal dari transaksi disbursement. Proses ini membutuhkan nomor handphone pengguna AstraPay dan juga jumlah saldo disbursement. Proses ini wajib dilakukan sebelum melakukan commit transaksi disbursement dan saldo AstraPay belum diterima oleh pengguna pada tahap ini.*

**Complete Code**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/disbursement-service/h2h/inquiries' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJiOThDaFhUWFVJQ3J4SjhadWt5OWNCUXpBLThqQnhxa0lndmpoM0h3SVpFIn0.eyJleHAiOjE2NDg2MjE2ODksImlhdCI6MTY0ODYyMTM4OSwianRpIjoiM2RlZmY3MzAtZTllZi00MWM5LTk4MWYtZDdlOTcyYjZiNjMzIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC42Ljc5OjgwODAvYXV0aC9yZWFsbXMvYXN0cmFwYXktYnVzaW5lc3MiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMmQzMGUzYjAtNjk0Yi00NjkyLWE4YmUtYmJhNTA5ZjBjNjQ5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiY2xpZW50SG9zdCI6IjE3Mi4yMC42LjEwOSIsImNsaWVudElkIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LTgxOTY5NjEyLWFmZjEtMTFlYy1iOTA5LTAyNDJhYzEyMDAwMiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNi4xMDkifQ.dcVfUrHe53BXZU2abOvSxFkZy59BcEO6Zb3jLMlU9FL_XBxN09lPSgQ_MY2ejksz0oTHkyjlRz9apG6c3n5GY34hj6jjY_UFV6A97VVv53F0nLqdpTlFKQYuzIrZs4BJmUzvBGviTI9K9dNagQ6rop6LwsqzJxGT8laS5EtIDDqaP4kciADqv0xOaWZfqYE9nbz91w54qQ5vT2y64XeINoQXFzr7hvhmXY8sgbCWDgKdlxnWU_oN-76EJmT2Yfa66aEMF8vIL7NLk_6_MsVduIdvge5covOltsWnICOZUHThfDj-CLbzUL55d62h7PkMmOKlCdl9HCS8pa_CKK4Rtg' \
--header 'x-signature: fb0a2ae94f0148452459dd1f070d00194c607bb5636e149e5fc271d0a7da825b' \
--header 'x-timestamp: 2021-04-21T11:37:51.436Z' \
--header 'x-astrapay-key: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--header 'Content-Type: application/json' \
--data-raw '{
    "userIdentification": "085939526552",
    "amount": 15000
}'
```

**Protocol**: HTTPS

**Method**: POST

**Sandbox URL**: https://sandbox.astrapay.com/disbursement-service/h2h/inquiries

### Request Header


| Field | Value | Wajib | Deskripsi |
| --- | --- | --- | --- |
| Authorization | Bearer Token | Y | Token yang didapatkan dari API proses [**Pembuatan Token Akses**](#pembuatan-token-akses). |
| x-signature | String | Y | Hasil generate dari [**AstraPay signature**](#signature). |
| x-timestamp | String | Y | Timestamp dengan format **ISO8601** dan dalam format waktu **UTC**, `yyyy-MM-ddTHH:mm:ss.SSSZ`. |
| x-astrapay-key | Bearer Token | Y | Token yang didapatkan dari API proses [**Pembuatan Token Akses**](#pembuatan-token-akses). |


```json
{
"userIdentification": 08123456789,
"amount": 15000
}
```

### Request Body


| Field | Tipe Data | Wajib | Deskripsi |
| --- | --- | --- | --- |
| userIdentification | String | Y | Nomor tujuan yang merupakan pengguna AstraPay |
| amount | Number | Y | Jumlah saldo yang akan ditransfer ke nomor tujuan |


### Respons


| Field | Tipe Data | Wajib | Deskripsi |
| --- | --- | --- | --- |
| inquiryCode | String | Y | 20 karakter alphanumeric yang merupakan kode inquiry dari transaksi disbursement. Yang didapatkan dari API [**Inquiry Disbursement**](#inquiry-disbursement). `inquiryCode` ini memiliki waktu selama **60 menit** sebelum **EXPIRED** |
| userIdentification | String | Y | Nomor tujuan yang merupakan pengguna AstraPay, diawali dengan `0` |
| amount | Number | Y | Jumlah saldo yang akan ditransfer ke nomor tujuan |
| serviceCharge | Number | Y | Jumlah biaya admin yang ada pada layanan disbursement |
| total | Number | Y | Total  = amount + serviceCharge |


**Contoh Respon
201 - Created**

```json
{
  "inquiryCode": "22021115093120333425",
  "userIdentification": "085939526552",
  "amount": 15000,
  "serviceCharge": 0,
  "total": 15000,
}
```

**400 - Bad Request**

```json
{
    "status": 400,
    "message": "Validation failed for disbursementInquiryRequestDto(userIdentification,). Error count 2",
    "error": "Bad Request",
    "path": "/disbursement-service/h2h/inquiries",
    "timestamp": "2022-03-01T15:14:20.323682200",
    "details": [
        {
            "code": "userIdentificationIsExist",
            "objectName": "disbursementInquiryRequestDto",
            "defaultMessage": "User identification is not registered as AstraPay user.",
            "field": "userIdentification",
            "rejectedValue": "08007007077"
        },
        {
            "code": "BalanceNotExceededLimitAfterInquiry",
            "objectName": "disbursementInquiryRequestDto",
            "defaultMessage": "Inquiry Amount exceeded account balance limit.",
            "field": "",
            "rejectedValue": {
                "userIdentification": "08007007077",
                "amount": 2000000
            }
        }
    ]
}
```

**400 - Bad Request (Invalid signature)**

```text
Invalid signature
```

### Kode Respons


| Status Code | Details | Deskripsi |
| --- | --- | --- |
| 201   **Created** |  | Inquiry transaksi disbursement berhasil terbuat. |
| 400   **Bad Request** | User identification is not registered as AstraPay user. | `userIdentification` tidak terdaftar sebagai pengguna AstraPay |
|  | Phone number / email is invalid! | Format value `userIdentification` tidak sesuai dengan format nomor handphone / email |
|  | Inquiry Amount exceeded account balance limit. | `amount` pada inquiry transaksi melebihi limit akun tujuan disbursement |
| 401   **Unauthorized** |  | `access_token` tidak sesuai atau kedaluwarsa |
| 500   **Internal Server Error** |  | Respons tidak valid dari Back-end **atau** Kesalahan Tidak Terdefinisi |


> [!WARNING]
> Untuk melakukan operasi API ini, Anda harus diautentikasi melalui salah satu metode berikut :
> [Bearer Token](#pembuatan-token-akses)

## Payment Disbursement

*Tahap ini merupakan tahap akhir dari disbursement, dengan menyertakan inquiryCode yang didapat dari proses Inquiry Disbursement dan notes yang juga dapat disertakan, maka saldo AstraPay akan diterima oleh pengguna AstraPay sesuai dengan inquiry disbursement dan notes yang disertakan (jika ada).*

**Complete Code**

```shell
curl --location --request POST 'https://sandbox.astrapay.com/disbursement-service/disbursements' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJiOThDaFhUWFVJQ3J4SjhadWt5OWNCUXpBLThqQnhxa0lndmpoM0h3SVpFIn0.eyJleHAiOjE2NDg2MjI2MjAsImlhdCI6MTY0ODYyMjMyMCwianRpIjoiYzRlOTVjMmMtMzE0OS00ZTE2LWExOWEtMjgyYWZhMjUzOWY5IiwiaXNzIjoiaHR0cDovLzE3Mi4yMC42Ljc5OjgwODAvYXV0aC9yZWFsbXMvYXN0cmFwYXktYnVzaW5lc3MiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiMmQzMGUzYjAtNjk0Yi00NjkyLWE4YmUtYmJhNTA5ZjBjNjQ5IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwiYWNyIjoiMSIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJkZWZhdWx0LXJvbGVzLWFzdHJhcGF5LWJ1c2luZXNzIiwib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoicHJvZmlsZSBlbWFpbCIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwiY2xpZW50SG9zdCI6IjE3Mi4yMC42LjEwOSIsImNsaWVudElkIjoiODE5Njk2MTItYWZmMS0xMWVjLWI5MDktMDI0MmFjMTIwMDAyIiwicHJlZmVycmVkX3VzZXJuYW1lIjoic2VydmljZS1hY2NvdW50LTgxOTY5NjEyLWFmZjEtMTFlYy1iOTA5LTAyNDJhYzEyMDAwMiIsImNsaWVudEFkZHJlc3MiOiIxNzIuMjAuNi4xMDkifQ.l5TATpndqlC8ac5TiT2bSqKQV-1kI97t260DFzLXpTkWK-S0CjjbdVW4zM3qPIpEztzLwXfTfFZb8DdzBRWDaZZcT0nbOl4R6886-2UfXZoAdknF1xZhdQazf-U8kpCjcd9E6Wt4s3n2evabjuBGvcSpbQlnOlUKTelOUVtgegpn0GbvbGau_0cgSC2-oHhWJiFZV4YCuvLRTEukb4Gbo0yFHhlzX-Vibz1CcWa1Og_OLFMkOpOx9UGNXmGR55F4_E7vWgW126hGyKNhBek6wtvRKtPf_5NLfqgtDghr_19YukTztVhlvaknb818j3E8TcSR_b_pmdrGP10S-9vc5g' \
--header 'x-signature: 9c4ba8bf3e2b0e66f4e48884e08dccbf0cfd533a6d02d871fb7a8b4914e93b9c' \
--header 'x-timestamp: 2021-04-21T11:37:51.436Z' \
--header 'x-astrapay-key: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--header 'Content-Type: application/json' \
--data-raw '{
  "inquiryCode": "22021115093120333425",
  "notes": "Disbursement Saldo AstraPay"
}'
```

**Protocol**: HTTPS

**Method**: POST

**Sandbox URL**: https://sandbox.astrapay.com/disbursement-service/h2h/disbursements

### Request Header


| Field | Value | Wajib | Deskripsi |
| --- | --- | --- | --- |
| Authorization | Bearer Token | Y | Token yang didapatkan dari API proses [**Pembuatan Token Akses**](#pembuatan-token-akses). |
| x-signature | String | Y | Hasil generate dari [**AstraPay signature**](#signature). |
| x-timestamp | String | Y | Timestamp dengan format **ISO8601** dan dalam format waktu **UTC**, `yyyy-MM-ddTHH:mm:ss.SSSZ`. |
| x-astrapay-key | Bearer Token | Y | Token yang didapatkan dari API proses [**Pembuatan Token Akses**](#pembuatan-token-akses). |


```json
{
"inquiryCode": 22021115093120333425,
"notes" : "Disbursement Saldo AstraPay"
}
```

### Request Body


| Field | Tipe Data | Wajib | Deskripsi |
| --- | --- | --- | --- |
| inquiryCode | String | Y | 20 karakter alphanumeric yang merupakan kode inquiry dari transaksi disbursement.Yang didapatkan dari API [**Inquiry Disbursement**](#inquiry-disbursement). `inquiryCode` ini memiliki waktu selama **60 menit** sebelum **EXPIRED** |
| notes | String | N | Catatan yang akan disertakan dalam transaksi disbursement dan akan masuk ke aplikasi AstraPay. **Tidak dapat menyertakan link ataupun HTML** |


Berikut tampilan dalam aplikasi AstraPay sesuai dengan transaksi disbursement yang dilakukan :
![img](./assets/detail-disbursement-f9dd5277.png)

### Respons


| Field | Tipe Data | Wajib | Deskripsi |
| --- | --- | --- | --- |
| disbursementNumber | String | Y | Kode transaksi disbursement yang sudah diproses |
| userIdentification | String | Y | Nomor tujuan yang merupakan pengguna AstraPay |
| amount | Number | Y | Jumlah saldo yang akan ditransfer ke nomor tujuan |
| serviceCharge | Number | Y | Jumlah biaya admin yang ada pada layanan disbursement |
| total | Number | Y | Total  = amount + serviceCharge |
| status | String | Y | Status transaksi disbursement |


**Contoh Respon
201 - Created**

```json
{
  "disbursementNumber": "INV/DIS/EXO/20220228/093744HQH",
  "userIdentification": "08123456789",
  "amount": 15000,
  "serviceCharge": 0,
  "total": 15000,
  "status": "SUCCESS",
}
```

**400 - Bad Request**

```json
{
    "status": 400,
    "message": "Validation failed for executeH2HDisbursementInquiryRequestDto(inquiryCode). Error count 1",
    "error": "Bad Request",
    "path": "/disbursement-service/disbursements",
    "timestamp": "2022-03-01T15:11:58.814281600",
    "details": [
        {
            "code": "H2HInquiryCodeIsNotExpired",
            "objectName": "executeH2HDisbursementInquiryRequestDto",
            "defaultMessage": "Inquiry code is expired.",
            "field": "inquiryCode",
            "rejectedValue": "22022307263351277906"
        }
    ]
}
```

**400 - Bad Request (Invalid Signature)**

```text
Invalid signature
```

Berikut penjelasan dari `status` inquiryCode:

- **"PENDING"** : Transaksi disbursement **belum** terproses, saldo belum bertambah ke akun AstraPay.
- **"PROCESSING"** : Transaksi disbursement sedang diproses, saldo belum bertambah ke akun AstraPay.
- **"SUCCESS"** : Transaksi disbursement berhasil dilakukan, saldo telah bertambah ke akun AstraPay.
- **"FAILED"** : Transaksi disbursement gagal dilakukan, saldo tidak bertambah ke akun AstraPay.
- **"EXPIRED"** : Transaksi disbursement sudah melewati **1 jam** setelah inquiry dibuat.

### Kode Respons


| Status Code | Details | Deskripsi |
| --- | --- | --- |
| 201   **Created** |  | Transaksi disbursement berhasil dilaksanakan. Advice payment disbursement (jika request dengan inquiryCode yang sama) |
| 400   **Bad Request** | Inquiry code is expired. | `inquiryCode` telah expired (melebihi 1 jam dari pembuatan inquiryCode). |
| 401   **Unauthorized** |  | `access_token` tidak sesuai atau kedaluwarsa |
| 500   **Internal Server Error** |  | Respons tidak valid dari Back-end **atau** Kesalahan Tidak Terdefinisi |


> [!WARNING]
> Untuk melakukan operasi API ini, Anda harus diautentikasi melalui salah satu metode berikut :
> [Bearer Token](#pembuatan-token-akses)

## Check Disbursement Status

*API ini digunakan untuk mendapatkan detail transasksi disbursement sesuai dengan inquiryCode yang disertakan. Sehingga, API akan memberikan respons data dari transaksi disbursement yang sudah dilaksanakan.*

**Complete Code**

```shell
curl --location --request GET 'https://sandbox.astrapay.com/disbursement-service/h2h/22092709523273159544' \
--header 'x-signature: 1e6d0d03132f20b774b93258720b00bc6a767d45c8bfd3418fc2c5fc5c395e41' \
--header 'x-timestamp: 2021-04-21T11:37:51.436Z' \
--header 'x-astrapay-key: eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJlVm9kSlYzeVN6aUlyLW9aZjZCQzJtUVVaTmJFLWFaQndETjFUUmxmZE1jIn0.eyJleHAiOjE2Mzk0NDY3NzksImlhdCI6MTYzOTQ0NjQ3OSwianRpIjoiMzUxYWVlYTAtNTE2Ny00OTVkLWJhNTYtYWY5NmRhOTg2ZTFjIiwiaXNzIjoiaHR0cDovLzE3Mi4yMC4zLjEyNDo4MDgwL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjJlNjJmNzVhLTA2Y2UtNGQ5ZC04NTRkLTdiYWYxYjk0OWJiOSIsInR5cCI6IkJlYXJlciIsImF6cCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJlbWFpbF92ZXJpZmllZCI6ZmFsc2UsImNsaWVudEhvc3QiOiIxNzIuMjAuMy4xMjMiLCJjbGllbnRJZCI6IjYwMDY4YzlmLTkyMGMtNGVlMy05M2Q5LWQ0NWRlMzcxZjJlMSIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC02MDA2OGM5Zi05MjBjLTRlZTMtOTNkOS1kNDVkZTM3MWYyZTEiLCJjbGllbnRBZGRyZXNzIjoiMTcyLjIwLjMuMTIzIn0.THD7R67KhJu93x4pBzQUSbxb4riGTROdPCFiVTGhz9v1sYmo7Ku1rDNrRw2hZcnnIxrXMPlVsHeLSrjS_gpXFUTSibqGlpqKpQ-niHxWnzHns-IbUAZF0Us0pj_7Yx3g0Q7huGwyV-ZyMXYrgZFCul4IKloUgEkYASznGGM2dCuH9kp90tGwXfQwLsgmq570mtE9LpSS0lIg92uLd8H7nDTCAhQVgG-9r5RkuxJv7GZqcajD2G4hrZgkR8FrtY5gnMu-mA4GzWGfpVwFWqRY_i9y2_nP5attQ07IsbFaARR6SKnMUAVG11mtRopDn8b4MTLpcAJ_IpVYRaQ8KcrZ_Q' \
--header 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI1QjhXRGtYSzVBMWpyeFVrckMyWnB4NFN4XzVBRUlhMVpjM1NsOVZobUtJIn0.eyJleHAiOjE2NjQyNjgxNjcsImlhdCI6MTY2NDI0NjU2NywianRpIjoiNTg2OTJiZjgtMWJiMS00ZTYxLWE3ODUtNWM0NmUxNzc1ZGE0IiwiaXNzIjoiaHR0cDovLzEwLjIwLjcuNjo4NDQzL2F1dGgvcmVhbG1zL2FzdHJhcGF5LWJ1c2luZXNzIiwiYXVkIjoiYWNjb3VudCIsInN1YiI6IjNkMDYwMjEyLWE5ODYtNDk5NC04MWFhLTE3NWM5NzRlYzJiZiIsInR5cCI6IkJlYXJlciIsImF6cCI6ImE5NjUyOTczLTRhNDMtNDM0Zi05MjRmLWY1ODRmNTBmZWVkNiIsImFjciI6IjEiLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsiZGVmYXVsdC1yb2xlcy1hc3RyYXBheS1idXNpbmVzcyIsIm9mZmxpbmVfYWNjZXNzIiwidW1hX2F1dGhvcml6YXRpb24iXX0sInJlc291cmNlX2FjY2VzcyI6eyJhY2NvdW50Ijp7InJvbGVzIjpbIm1hbmFnZS1hY2NvdW50IiwibWFuYWdlLWFjY291bnQtbGlua3MiLCJ2aWV3LXByb2ZpbGUiXX19LCJzY29wZSI6InByb2ZpbGUgZW1haWwiLCJjbGllbnRIb3N0IjoiMDowOjA6MDowOjA6MDoxIiwiZW1haWxfdmVyaWZpZWQiOmZhbHNlLCJjbGllbnRJZCI6ImE5NjUyOTczLTRhNDMtNDM0Zi05MjRmLWY1ODRmNTBmZWVkNiIsInByZWZlcnJlZF91c2VybmFtZSI6InNlcnZpY2UtYWNjb3VudC1hOTY1Mjk3My00YTQzLTQzNGYtOTI0Zi1mNTg0ZjUwZmVlZDYiLCJjbGllbnRBZGRyZXNzIjoiMDowOjA6MDowOjA6MDoxIn0.bCxML3cttzp8sIza_T0YaSsn_rM5EknvNLh8xocgwWePRBd8rqTlMvvWbfdu-jktGZqAVXcy7z8tdkI4o4g5gbAZov86T_JS4rtYABIMD0GzGlyUV2MDI5_0gmRLh0Pvr5Qx_6yNlJl_3rne6bnjruWeWqRAlYAsKbrlGj56Nzjii3FeGO9qrKECx3S1oZl2MJzSD0tbDjSwEIfgHTtZgVdWUSsX9lB2F8hPJC4TpVMKEIHgveGBxqir1qwDZzfEMqqtYEABeep0AfpB_GE9QHVBn0DSwZoxc8gva0DV-ttrNZknkLSi-AyT_5zPrXwM8l0CzFLpvsHKZCksV2VDhQ' \
```

**Protocol**: HTTPS

**Method**: GET

**Sandbox URL**: https://sandbox.astrapay.com/disbursement-service/h2h/{inquiryCode}

### Request Header


| Field | Value | Wajib | Deskripsi |
| --- | --- | --- | --- |
| Authorization | Bearer Token | Y | Token yang didapatkan dari API proses [**Pembuatan Token Akses**](#pembuatan-token-akses). |
| x-signature | String | Y | Hasil generate dari [**AstraPay signature**](#signature). Dengan value **request body** berupa **"{}"** |
| x-timestamp | String | Y | Timestamp dengan format **ISO8601** dan dalam format waktu **UTC**, `yyyy-MM-ddTHH:mm:ss.SSSZ`. |
| x-astrapay-key | Bearer Token | Y | Token yang didapatkan dari API proses [**Pembuatan Token Akses**](#pembuatan-token-akses). |


### Respons


| Field | Tipe Data | Wajib | Deskripsi |
| --- | --- | --- | --- |
| disbursementNumber | String | Y | Kode transaksi disbursement yang sudah diproses |
| userIdentification | String | Y | Nomor tujuan yang merupakan pengguna AstraPay |
| amount | Number | Y | Jumlah saldo yang akan ditransfer ke nomor tujuan |
| serviceCharge | Number | Y | Jumlah biaya admin yang ada pada layanan disbursement |
| total | Number | Y | Total  = amount + serviceCharge |
| status | String | Y | Status transaksi disbursement |


**Contoh Respon
201 - Created**

```json
{
  "disbursementNumber": "INV/DIS/EXO/20220228/093744HQH",
  "userIdentification": "08123456789",
  "amount": 15000,
  "serviceCharge": 0,
  "total": 15000,
  "status": "SUCCESS",
}
```

**400 - Bad Request**

```json
{
    "status": 400,
    "message": "Disbursement transaction has not been committed.",
    "error": "Bad Request",
    "path": "/disbursement-service/h2h/1234512121",
    "timestamp": "2022-09-09T08:37:57.912921800",
    "details": []
}
```

**400 - Bad Request (Invalid Signature)**

```text
Invalid signature
```

Berikut penjelasan dari `status` inquiryCode:

- **"PENDING"** : Transaksi disbursement **belum** terproses, saldo belum bertambah ke akun AstraPay.
- **"PROCESSING"** : Transaksi disbursement sedang diproses, saldo belum bertambah ke akun AstraPay.
- **"SUCCESS"** : Transaksi disbursement berhasil dilakukan, saldo telah bertambah ke akun AstraPay.
- **"FAILED"** : Transaksi disbursement gagal dilakukan, saldo tidak bertambah ke akun AstraPay.
- **"EXPIRED"** : Transaksi disbursement sudah melewati **1 jam** setelah inquiry dibuat.

### Kode Respons


| Status Code | message | Deskripsi |
| --- | --- | --- |
| 200   **OK** |  | InquiryCode yang disertakan terdaftar. |
| 400   **Bad Request** | Disbursement transaction has not been committed. | `inquiryCode` belum melaksanakan tahapan payment disbursement |
| 400   **Bad Request** | The inquiry code does not belong to the merchant. | Host merchant melakukan request dengan `inquiryCode` dari merchant yang berbeda. |
| 401   **Unauthorized** |  | `access_token` tidak sesuai atau kedaluwarsa |
| 500   **Internal Server Error** |  | Respons tidak valid dari Back-end **atau** Kesalahan Tidak Terdefinisi |


> [!WARNING]
> Untuk melakukan operasi API ini, Anda harus diautentikasi melalui salah satu metode berikut :
> [Bearer Token](#pembuatan-token-akses)
