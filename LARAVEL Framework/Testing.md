Laravel sekarang (11.x), menggunakan dua testing framework yaitu [PHPUnit](https://phpunit.de/index.html) dan [Pest](https://pestphp.com/), yang bisa kita pilih. Tetapi untuk sekarang saya lebih prefer menggunakan PHPUnit.

Secara garis besar, di Laravel terdapat 2 jenis test, yaitu:
1. **Unit Test**: Untuk membuat test kita bisa membuat class unit test seperti biasanya, yaitu dengan membuat class turunan dari `PHPUnit\Framework\TestCase`. Jika kita perlu membuat test tanpa harus menggunakan fitur Laravel (misalnya kita membuat class atau function yang tidak ada hubungannya dengan Laravel), maka kita cukup membuat Unit Test saja.
2. **Feature Test / Integration Test**: Jika kita butuh fitur Laravel untuk testing (misalnya kita membuat routing, service provider, controller), maka kita perlu atau wajib membuat Feature Test atau Integration Test. Bedanya dari Uni Test adalah di Integration Test, fitur-ditur Laravel bisa diakses dengan mudah, misal kita mau memanggil Database, Controller dan lain-lain. Untuk membuat Integation Test kita perlu membuat class turunan dari `Illuminate\Fondation\Testing\TestCase`. Integration test akan lebih lambat dibandingkan Unit Test, karena perlu me-load framework Laravel terlebih dahulu.

## Membuat Test

Untuk membuat  test kita bisa lakukan manual, dengan `extend` class yang disebutkan sebelumnya, atau buat otomatis dengan menggunakan artisan.

Untuk membuat Integration Test gunakan perintah:
```bash
php artisan make:test NamaTest
```
Maka test secara otomatis akan masuk ke directory `tests/Feature`.

Untuk membuat Unit Test gunakan opsi `--unit` seperti ini:
```bash
php artisan make:test NamaTest --unit
```
Maka test secara otomatis akan masuk ke directory `tests/Unit`.

## Menjalankan Test

Untuk menjalankan test kita bisa jalankan langsung binary PHPUnit yang ada di vendor seperti biasa, atau menggunakan perintah:
```bash
php artisan test
```

