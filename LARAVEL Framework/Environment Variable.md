Environment variable adalah tempat dimana kita menyimpan nilai konfigurasi untuk aplikasi kita (seperti konfigurasi untuk terkoneksi ke database, dll). Environment variable sangat cocok untuk konfigurasi yang memang butuh berubah-ubah nilainya sesuai dengan environment (lingkungan) dimana aplikasi dijalankan. Misalnya konfigurasi database seperti username dan password yang biasanya berbeda antara local dan production.

Ada banyak opsi untuk menerapkan environment variable, kita bisa menambahkannya pada sistem operasi kita, atau disebut sebagai *system-level environment variable*, atau pada server web kita, yang disebut dengan *server-level environment variable*, atau bisa juga pada file `.env`, tetapi khusus untuk file `.env` kita harus menambahkannya pada `.gitignore` dan membuat file `.env.example` (nama bebas) yang isinya sama seperti pada file `.env`, tetapi tidak memiliki value, alias cuman key atau nama environment variable nya saja dan file ini tidak ditambahkan ke `.gitignore`.

Internal implementasi dari Environment variable di Laravel menggunakan library [PHP dotenv](https://github.com/vlucas/phpdotenv), library ini berfungsi untuk me-load environment variable dari file `.env` ke `getenv()`, `$_ENV` dan `$_SERVER`.

Di Laravel untuk mengakses atau mengambil data dari environment variable, baik itu yang terdapat pada file `.env`, dari server-level, ataupun dari system-level environment variable, kita bisa menggunakan function `env` yang telah disediakan oleh Laravel.

## Default Value

Laravel mendukung default value untuk environment variable, default value adalah nilai yang akan digunakan ketika environment variable yang kita ambil tidak tersedia. Kita bisa menggunakan function `env(key, default)`.