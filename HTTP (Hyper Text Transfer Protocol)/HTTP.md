
Merupakan protokol untuk melakukan transmisi (pengiriman) hypermedia document, seperti HTML JavaScript, CSS, Image, Audio, Video dan lain-lain, antara satu komputer dengan komputer yang lain. HTTP awalnya didesign untuk komunikasi antara Web Server dan Browser, namun saat ini sering juga digunakan untuk kebutuhan lain.

Intinya HTTP itu adalah hanya sebuah spesifikasi atau aturan.

## Client Server

HTTP mengikuti arsitektur client dan server. Client mengirim HTTP Request untuk meminta atau mengirim informasi ke Server, Server membalasnya dengan HTTP Response dari HTTP Request yang diterima.

## Stateless

HTTP merupakan protokol yang stateles, artinya tiap HTTP Request merupakan request yang independen, tidak ada keterkaitan atau hubungan dengan HTTP Request sebelum atau setelahnya. Ini dilakukan agar HTTP request tidak harus dilakukan dengan sequence (berurutan), misal harus buka halaman 1 dulu, baru halaman 2, HTTP tidak seperti itu, kita bebas mau buka halaman apapun. Sehingga dengan HTTp yang stateless, Client bisa melakukan HTTP Request secara bebas tanpa ada aturan harus dimulai dari mana.

## Session

Jika HTTP adalah stateless, lalu bagaimana dengan session? misal client harus login dulu sebelum berinteraksi.

Untuk menangani masalah seperti ini HTTP memiliki fitur yang namanya HTTP Cookie, HTTP Cookie memaksa Client untuk menyimpan informasi yang disimpan oleh server.

## HTTPS

Secara default HTTP itu tidak aman, maka sekarang website banyak menggunakan HTTPS, yang merupakan HTTP dengan enkripsi. Pada HTTPS menggunakan SSL (Secure Sockets layer) untuk melakukan enkripsi HTTP Request dan HTTP Response, sehingga HTTPS lebih aman dibandingkan HTTP biasa.

## HTTP Terminology

Istilah-istilah dalam HTTP

### TCP (Transmission Control Protocol)

Adalah salah satu protocol dalam jaringan komputer yang biasa digunakan oleh web, email atau lainnya. HTTP itu menggunakan TCP untuk komunikasi dari Client ke Servernya. Ketika menggunakan jaringan internet, kemungkinan besar kita menggunakan protocol TCP untuk melakukan koneksi jaringannya

### IP (Internet Protocol)

IP digunakan sebagai identitas komputer di jaringan. Jadi saat kita terkoneksi ke jaringan atau internet, pasti memiliki IP. Setiap komputer baik itu Client dan Server akan memiliki IP. IP adalah alamat komputer kita di jaringan.

### DNS (Domain Name Server)

DNS merupakan tempat yang berisi data katalog pemetaan antara nama Domain di URL menuju lokasi IP komputer.

Saat Browser mengakses sebuah Domain di web, sebenarnya prosesnya akan bertanya ke DNS untuk mendapatkan IP, lalu Browser akan melakukan Request ke IP tersebut.

### Web Server

Merupakan aplikasi yang berjalan di jaringan internet yang bertugas sebagai server. Web server berisi data dan informasi yang biasa di akses oleh Client (Web Browser). Web Server akan menerima request dari Client dan membalas request tersebut berupa informasi yang diminta oleh Client, kalau kita buka YouTube, yang dikirim adalah Video misalnya, atau kalau kita buka Sotify yang dikirim adalah audio.

## HTTP Flow

### Server

Merupakan sebuah komputer, dimana semua informasi disimpan pada komputer tersebut. Komputer server biasanya menjalankan aplikasi Web Server agar bisa menerima protocol HTTP.

### Client

Client merupakan komputer yang bertugas mengirim HTTP Request ke komputer Server. Untuk mengirim request HTTP, biasanya client akan menggunakan aplikasi Web Browser.

Client dan Server harus terkoneksi dalam jaringan yang sama, agar bisa berkomunikasi.

Misal saja, client dan server terhubung dalam jaringan internet.

### Request

Client akan mengirim request ke Server dalam bentuk HTTP Request. HTTP Request berisikan informasi seperti lokasi resource, data yang dikirim jika ada dan lain-lain.

HTTP Request akan diterima oleh Server.

Selanjutnya Server akan memproses Request yang diminta oleh Client tersebut.

### Response

Setelah Server memproses HTTP Request yang dikiirim oleh Client, Server akan membalas dengan HTTP Response, HTTP Response biasanya berisikan data yang diminta oleh Client dalam HTTP Request.

## HTTP Message

HTTP Request dan HTTP Response, sebenarnya adalah sebuah HTTP Message. HTTP Message memiliki standarisasi format.

![Diagram HTTP Flow](https://ik.imagekit.io/rezafikkri/Diagram%20HTTP%20Flow.png?updatedAt=1732871271228)
<small style="color:gray">Diagram HTTP Flow</small>

## HTTP Method

| **HTTP Method** | **Keterangan**                                                                                                                                                                                                                                              |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GET             | GET method digunakan untuk melakukan request data. Request menggunkan GET hanya untuk menerima data, bukan untuk mengirim data.                                                                                                                             |
| HEAD            | HEAD method digunakan seperti GET, tapi tanpa membutuhkan response body.                                                                                                                                                                                    |
| POST            | POST digunakan untuk mengirim data ke server, POST biasanya digunakan untuk mengirim data baru, sehingga biasanya memiliki request body.                                                                                                                    |
| PUT             | PUT method digunakan untuk mengganti semua data yang terdapat di Server dengan data baru yang dikirim dari request.                                                                                                                                         |
| DELETE          | DELETE method digunakan untuk menghapus data.                                                                                                                                                                                                               |
| PATCH           | PATCH method digunakan untuk mengubah sebagian data.                                                                                                                                                                                                        |
| OPTIONS         | OPTIONS method digunakan untuk mendeskripsikan opsi komunikasi yang tersedia. Untuk bertanya kepada Server opsi komunikasi/opsi operasi apa saja yang tersedia di Server tersebut, apakah ada GET, POST dan sebagainya.                                     |
| TRACE           | TRACE method merupakan request method untuk debugging. Response TRACE method akan mengembalikan seluruh informasi yang dikirim oleh Client. Saat membuat web sangat direkomendasikan untuk tidak mengaktifkan TRACE method ketika sudah live di production. |

## URL (Uniform Resource Locator)

Merupakan alamat dari sebuah resource di web.
![Anatomy URL](https://ik.imagekit.io/rezafikkri/Anatomy-URL.svg?updatedAt=1732871271291)
- **Schema**: mengindikasikan protocol yang perlu digunakan oleh Client,
- **Authority**: terdiri dari nama Domain dan nomor Port yang dipisahkan dengan titik dua (:)
    - Nama domain nantinya akan ditanyakan ke DNS untuk mendapatkan alamat IP nya, namun juga bisa langsung menggunakan IP jika website tersebut tidak memiliki Domain.
    - Nomor port tidak wajib jika web server menggunakan standar port dari HTTP protocol (80 untuk HTTP dan 443 untuk HTTPS), jadi jika kita bisanya mengakses [https://google.com](https://google.com), itu sebenarnya secara default Browser akan menggunakan port 443. Tetapi jika nomor port nya berbeda, misalnya nomor port nya 8000, maka wajib memasukkan nomor port tersebut.
- **Path**: berisi informasi menuju ke resource yang kita tuju dan path tidak wajib. Path dulunya berisi lokasi file fisik yang ada di server, tetapi sekarang path umunya berisi sebuah _abstraksi_ yang dihandle oleh web server, tanpa ada lokasi file fisik apapun seperti dulu. Contoh: `https://rezafikkri.github.io/blogs/css-combinator-cara-kerja-dan-penggunaannya`.
- **Parameters**: merupakan informasi tambahan yang berisi `key=value`, yang diawali oleh tanda `?` setelah Authority atau Path, yang antar parameter dipisah oleh tanda `&` dan parameters tidak wajib.
- **Anchor**: merupakan representasi bookmark (semacam penanda) dalam sebuah halaman website, yang mana anchor ini memberikan arah kepada browser untuk menampilkan konten yang terletak pada lokasi yang ditandai tersebut. Mungkin kamu pernah mengakses sebuah halaman website, yang terdapat anchor pada URL-nya, maka secara otomatis Browser akan melakukan scroll ke tempat dimana anchor tersebut di definisikan, untuk contoh nya kamu bisa buka halaman [Anchor MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL#anchor).

## HTTP Header

Merupakan informasi tambahan yang biasa dikirim di Request atau Response. HTTP header biasanya digunakan agar informasi tidak harus dikirim melalui Request Body atau Response Body. HTTP Header berisi key : value dan saat ini sudah banyak sekali standarisasi nama key pada HTTP Header.

| **HTTP Header** | **Keterangan**                                            |
| --------------- | --------------------------------------------------------- |
| Host            | Authority pada URL (wajib sejak versi HTTP/1.1)           |
| Content-Type    | Tipe data dari HTTP body                                  |
| User Agent      | Informasi user-agent (seperti browser dan system operasi) |
| Accept          | Tipe data yang diterima oleh client                       |
| Authorization   | Credential untuk autentikasi (misal username + password)  |
HTTP Header digunakan sebagai informasi tambahan diluar URL dan Body.

## HTTP Status

Merupakan kode HTTP Response yang mengindikasikan apakah sebuah HTTP request telah berhasil diselesaikan.

HTTP status dibagi dalam beberapa grup:
1. **Informational responses** (`100` – `199`): Mengindikasikan bahwa request telah diterima dan dimengerti. Namun client diminta menunggu tahapan akhir response. Pada kenyataannya informational respone sangat jarang sekali digunakan.
2. **Successful responses** (`200` – `299`): Mengindikasikan bahwa request telah diterima, dimengerti dan sukses diproses oleh server.
3. **Redirection messages** (`300` – `399`): Mengindikasikan bahwa client harus melakukan aksi selanjutnya untuk menyelesaikan request. Biasanya redirect status digunakan ketika lokasi sebuah resource berubah, sehingga server meminta client untuk berpindah ke URL lain. Contohnya adalah pada sistem login, setelah login berhasil maka kita akan dialihkan ke halaman lain (dashboard, dsb).
4. **Client error responses** (`400` – `499`): Mengindikasikan bahwa request yang dikirim oleh client tidak diterima oleh server dikarenakan request yang dikirim diangap tidak valid. Contohnya client mengirim body yang salah, client melakukan request ke tanpa autentikasi di resource yang mewajibkan autentikasi, dan lain-lain.
5. **Server error responses** (`500` – `599`): Mengindikasikan bahwa terjadi kesalahan di server, biasanya ini terjadi ketika ada masalah di server, seperti misalnya tidak terkoneksi ke basis data, terdapat jaringan error di server, dan lain-lain.

## HTTP Body

HTTP Body merupakan data yang bisa dikirim di HTTP request, atau data yang diterima dari HTTP response. Artinya client bisa mengirim data ke server menggunakan HTTP Body, begitu juga sebaliknya, server bisa memberikan data di response menggunakan HTTP Body.

### Content-Type

HTTP Body erat kaitannya dengan header key Content-Type. Biasanya agar client dan server mudah mengerti isi dari HTTP Body, HTTP Message akan memiliki header Content-Type, yang berisi informasi tipe data HTTP Body.

## HTTP Caching

Caching adalah menyimpan data di client sampai batas waktu yang sudah ditentukan, sehingga jika client ingin melakukan request resource yang sama, cukup ambil resourcenya di client, tanpa harus meminta ulang ke server.

HTTP Caching sangat cocok dilakukan utnuk resource file static yang jarang berubah, seperti file gambar, audio, video dan lain-lain.

![Diagram HTTP Caching](https://ik.imagekit.io/rezafikkri/HTTP%20Caching%20diagram.png?updatedAt=1732874411133)
### Header Cache Control

Server ketika meminta agar client melakukan caching, maka HTTP Response perlu menambahkan informasi Cache-Control di header.

Cache-Control berisi seperti berapa lama client bisa menyimpan response tersebut, sehingga tidak perlu meminta ulang ke Server. 