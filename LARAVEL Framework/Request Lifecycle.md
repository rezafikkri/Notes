
## First Steps

Entry point pertama dari aplikasi Laravel adalah file `index.php` yang terdapat di folder `public`. Semua request yang masuk ke aplikasi Laravel maka akan masuk melalui file ini.
File ini sengaja disimpan di dalam folder `public` tersendiri, agar file-file kode program lainnya tidak bisa diakses melalui URL.

## HTTP / Console Kernels

Dari `index.php` request akan dilanjutkan ke class kernel, yaitu HTTP kernel atau Console kernel. HTTP kernel digunakan untuk menangani request berupa HTTP, sedangkan Console kernel digunakan untuk menangani request berupa perintah console.

## Service Provider


