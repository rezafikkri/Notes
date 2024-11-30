
## First Steps

Entry point pertama dari aplikasi Laravel adalah file `index.php` yang terdapat di folder `public`. Semua request yang masuk ke aplikasi Laravel maka akan masuk melalui file ini.
File ini sengaja disimpan di dalam folder `public` tersendiri, agar file-file kode program lainnya tidak bisa diakses melalui URL.

## HTTP / Console Kernels

Dari `index.php` request akan dilanjutkan ke class kernel, yaitu HTTP kernel atau Console kernel menggunakan method `handleRequest` atau `handleCommand` dari application instance. HTTP kernel digunakan untuk menangani request berupa HTTP, sedangkan Console kernel digunakan untuk menangani request berupa perintah console.

HTTP kernel mendefinisikan array `bootstrappers` yang akan dijalankan sebelum request dieksekusi. `bootstrappers` ini mengkonfigurasi error handling, logging, mendeteksi environment dan melakukan pekerjaan-pekerjaan lain yang dibutuhkan sebelum request benar-benar di tangani.

HTTP kernel juga bertanggung jawab untuk memasukkan request melalui middleware. Middleware ini menangani pembacaan dan penulisan HTTP Session, menentukan apakah aplikasi dalam mode *maintenance*, memverifikasi CSRF Token dan lain-lain.

Pada intinya tugas dari method `handle` dari HTTP kernel itu sebenarnya simpel, yaitu menerima Request dan mengambalikan Response.

## Service Provider

Salah satu dari banyak aksi kernel bootstrapping adalah me-load Service provider. Service Provider adalah yang bertanggung jawab melakukan bootstrapping semua component di Laravel, seperti database, queue, validation dan routing component. Sebenarnya semua hal atau detail dari eksekusi request dilakukan oleh si Service Provider.

> Bootstrapping merujuk ke proses inisialisasi (pengisian nilai awal suatu suatu objek pemrograman atau variable)

Laravel akan melakukan iterasi semua Service Provider, melakukan proses registerasi dan bootstrapping untuk semua Service Provider.

Jadi urutannya adalah:
1. Request masuk ke `public/index.php`
2. Request masuk ke kernel
3. Kernel meregistrasi semua Service Provider
4. Setelah itu Request diserahkan ke Router untuk dikirim ke Controller

Pada dasarnya, setiap fitur utama yang ditawarkan oleh Laravel, dibootstrap dan dikonfigurasi oleh sebuah Service Provider.

Secara internal Laravel menggunakan puluhan service provider dan kita juga bisa membuat Service Provider kita sendiri.

## Routing

Ketika aplikasi sudah di bootstrap dan semua service provider sudah di registrasi, Request akan serahkan ke router untuk dikirim. Router akan mengirim request ke Controller, dan juga menjalankan middleware untuk Controller tertentu.

Middleware menyediakan cara yang mudah untuk memfilter request yang masuk ke aplikasi kita. Sebagai contoh laravel menyediakan middleware untuk meverifikasi apakah pengguna sudah login atau belum.

Jika request lolos dari semua middleware, method Controller akan dijalankan dan response yang dikembalikan oleh method Controller akan dikirim kembali melalui middleware 

