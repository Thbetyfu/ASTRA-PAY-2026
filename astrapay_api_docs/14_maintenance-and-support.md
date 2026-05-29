# Maintenance and Support


## Overview QRIS SDK

Dokumen ini menjelaskan kebijakan maintenance untuk penggunaan Software Development Kit (SDK) AstraPay QRIS. AstraPay secara berkala memperbarui SDK dan API yang terintegrasi untuk memperkenalkan fitur baru, melakukan perbaikan, dan melakukan pemeliharaan pada fitur yang sudah ada. Setiap versi QRIS SDK akan dipublikasikan di Maven (Android) dan CocoaPods (iOS) serta didokumentasikan di dokumentasi API kami.

**Partner SDK** adalah pihak yang menggunakan SDK QRIS AstraPay dengan mengintegrasikan layanan pembayaran QRIS AstraPay ke dalam aplikasi/brand. Kami sangat menyarankan partner SDK untuk tetap memantau perkembangan rilis SDK agar dapat memanfaatkan perkembangan fitur terbaru.

### Versioning

Versi rilis SDK AstraPay QRIS mengikuti format X.Y.Z (Major.Minor.Patch).

- Peningkatan versi major terjadi ketika terdapat perubahan signifikan pada SDK, seperti perubahan yang tidak backward compatible kepada API yang terintegrasi. Versi SDK sebelumnya tidak dapat digunakan, sehingga partner SDK harus memperbarui versi SDK ke yang terbaru. Kami mendorong partner SDK untuk berhati-hati dan melakukan pengujian yang seksama saat memperbarui major version SDK.
- Peningkatan versi minor dilakukan ketika ada penambahan fitur ataupun perubahan design yang tidak mengganggu fungsionalitas versi SDK sebelumnya.
- Peningkatan versi patch dilakukan untuk perbaikan yang backward compatible.

**Catatan** : Atas peningkatan versi minor maupun patch,  partner SDK tetap disarankan untuk menggunakan versi terbaru dan memperbarui versi SDK mereka dengan seksama.

### Phases



Setiap versi minor SDK AstraPay dibagi menjadi tiga fase:



- **Available**
- **Sunset: ** SDK telah mencapai akhir masa pakai dan tidak dapat lagi digunakan.
- **Deprecated: ** SDK masih dapat digunakan oleh partner SDK yang saat ini menggunakan versi tersebut. Setiap co-brand diharapkan untuk memperbarui ke versi SDK terbaru pada tanggal Sunset yang telah ditentukan. Co-brand baru disarankan untuk tidak mengintegrasikan versi SDK yang telah deprecated.

### Communication Method

Kami akan mengkomunikasikan rilis kami melalui beberapa metode:

- Pemberitahuan melalui email akan dikirimkan kepada PIC partner SDK untuk pembaruan major maupun minor versions.
- Dokumentasi rilis dapat ditemukan di https://astrapay.com/docs/api.

### Releases

### iOS

![Slate: API Documentation Generator](https://storage.googleapis.com/astrapay-prd-cdn/assets/apidocs/ios.png)

### Android

![Slate: API Documentation Generator](https://storage.googleapis.com/astrapay-prd-cdn/assets/apidocs/android.png)
