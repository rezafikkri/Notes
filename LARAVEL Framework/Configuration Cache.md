Untuk meningkatkan kecepatan aplikasi kita, kita sebaiknya meng-cache semua file konfigurasi kita ke dalam satu file dengan menggunakan perintah `php artisan config:cache`. Perintah tersebut juga sebaiknya dijalankan ketika dalam proses deployment ke production, tetapi perintah tersebut juga sebaiknya tidak dijalankan ketika sedang development, karena pastinya konfigurasi akan sering berubah.

> Cache adalah tempat penyimpanan sementara dari data dalam sebuah perangkat dan Caching adalah kegiatan menyimpan copy dari data kedalam sebuah cache, sehingga data yang di-cache bisa diakses dengan cepat.

Setelah perintah `php artisan config:cache` dijalankan, maka semua kofigurasi yang ada di folder `config` akan di merge atau dijadikan satu menjadi file `bootstrap/cache/config.php`.

Jika file cache sudah dibuat dan kita menambahkan konfigurasi pada di file php yang terdapat pada folder `config`, konfigurasi baru tersebut tidak akan bisa diakses, karena Laravel akan selalu menggunakan konfigurasi cache jika ada, oleh karena itu kita bisa membuat ulang cachenya, atau jika ingin menghapus cache-nya kita bisa menggunakan perintah:

```bash
php artisan config:clear
```

Selain itu juga pastikan kita hanya akan memanggil function `env` di dalam file konfigurasi yang terdapat di folder `config` saja, karena jika konfigurasi sudah di-cahce maka file `.env` tidak akan di-load, karena itu function `env` hanya akan mengembalikan external environment variable (system-level atau server-level).

> Menurut saya function `env` idealnya hanya di panggil di dalam file konfigurasi yang ada di folder `config` saja, kecuali jika memang ada kebutuhan untuk menggunakan function `env` di selain file konfigurasi, maka di production kita harus mengatur environment variablenya di system-level atau server-level.