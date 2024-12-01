Internal implementasi dari Environment variable di Laravel menggunakan library [PHP dotenv](https://github.com/vlucas/phpdotenv), library ini berfungsi untuk me-load environment variable dari file `.env` ke `getenv()`, `$_ENV` dan `$_SERVER`.

Di Laravel untuk mengakses atau mengambil data dari environment variable, baik itu yang terdapat pada file `.env`, dari server-level, ataupun dari system-level environment variable, kita bisa menggunakan function `env` yang telah disediakan oleh Laravel.

## Default Value

Laravel mendukung default value untuk environment variable, default value adalah nilai yang akan digunakan ketika environment variable yang kita ambil tidak tersedia. Kita bisa menggunakan function `env(key, default)`.

