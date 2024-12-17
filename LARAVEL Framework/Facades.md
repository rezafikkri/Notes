Facades adalah class yang menyediakan static akses ke class atau fitur yang tersedia di dalam Service Container atau Application. Laravel dilengkapi dengan banyak facades yang menyediakan hampir ke semua fitur Laravel.

Facades sangat membantu misalnya pada kasus ketika kita membuat kode class yang bukan bawaan fitur Laravel.

## Kapan Menggunakan Facades?

Gunakan facades jika memang dibutuhkan saja, jika bisa dilakukan menggunakan dependency injection, gunakan dependency injection.

Terlalu banyak menggunakan facades akan membuat kita tidak sadar bahwa sebuah class banyak sekali memiliki dependency, karena sebenarnya ketika kita menggunakan facades, kita akan membuat dependency (ketergantungan) ke tempat lain, sedangkan jika kita menggunakan dependency injection, kita bisa sadar dengan banyaknya parameter yang terdapat di constructor, tetapi kalau kita menggunakan facades tidak akan terlihat, karena kita menggunakan static function untuk mengakses dependencynya.

## Facades vs Helper Function

