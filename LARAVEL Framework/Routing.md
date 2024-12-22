Routing adalah proses menerima HTTP Request dan menjalankan kode sesuai dengan URL yang diminta. Routing biasanya tergantung pada HTTP Method dan URL.

## Basic Routing

Salah satu routing yang paling sederhana adalah menggunakan path dan closure   function untuk handlernya.

Kita bisa menggunakan Facades Route, lalu menggunakan function sesuai dengan HTTP Methodnya, misal:

```php
Route::get('/rezafikkri', function () {
    return 'RezaFikkri';
});
```

## Redirect

Router juga bisa digunakan untuk redirect dari satu halaman ke halaman lain. Kita bisa menggunakan function `Route::redirect(from, to)`.

## Melihat Semua Routing

Untuk melihatnya kita bisa menggunakan perintah:

```bash
php artisan route:list
```

## Fallback Route

Jika kita mengakses halaman yang tidak ada di aplikasi Laravel kita, maka otomatis akan mengembalikan erro 404. Kadang-kadang kita ingin mengubah tampilan error ketika halaman yang diakses tidak ada.

Pada kasus seperti ini kita bisa menggunakan fallback route, yaitu callback yang akan dieksekusi ketika tidak ada route yang cocok dengan halaman yang diakses. Kita bisa menggunakan function `Route::fallback(closure)`.