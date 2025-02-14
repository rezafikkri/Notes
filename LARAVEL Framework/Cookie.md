Saat membuat HTTP response, kadang kita perlu membuat cookie. Salah satu kegunaan Cookie yang sering kita gunakan adalah untuk management session di aplikasi berbasis web.

Secara default Cookie yang dibuat di Laravel akan selalu di enkripsi dan ketika kita membacanya pun akan secara otomatis di dekrip. Semua hal itu dikalukan secara otomatis oleh middleware `Illuminate\Cookie\Middleware\EncryptCookies`.

Jika kita ingin menonaktifkan enkripsi untuk bagian dari cookie yang dibuat oleh aplikasi kita, kita bisa menggunakan method `encryptCookies` di dalam file `bootstrap/app.php`:

```php
->withMiddleware(function (Middleware $middleware) {
	$middleware->encryptCookies(except: [
		'cookie_name',
	]);
})
```

## Membuat Cookie

Untuk membuat cookie kita bsia menggunakan method `cookie(name, value, minutes, path, domain, secure, httpOnly)` di object Response. Argument yang dimasukkan kedalam method tersebut sama persis seperti pada function [setcookie](https://www.php.net/manual/en/function.setcookie.php) pada PHP native, yang membedakan hanya pada argument untuk menentukan *expire* nya, dimana pada method `cookie` di Laravel, yang kita masukkan adalah menit, sedangkan yang ada pada function `setcookie` di PHP native, yang kita masukkan adalah Unix timestamp dalam bentuk second.

## Menerima Cookie

Setelah membuat Cookie di Response, secara otomatis Cookie akan disimpan di Brwoser sampai expire. Browser akan secara otomatis mengirim Cookie tersebut di domain dan path yang telah di tentukan ketika kita membuat Cookie. Oleh karena itu, kita bisa menangkap data Cookie  tersebut di Request dengan method `cookie(name, default)` atau jika kita ingin mengambil semua Cookie dalam array, kita bisa menggunakan method `$request->cookies->all()`.

## Clear Cookie

Untuk menghapus Cookie selain dengan secara manual menghapusnya melalui Browser, kita juga bisa menghapus Cookie dengan cara membuat Cookie dengan nama yang sama, nilainya kita kosongkan saja dan waktu kadaluarsanya kita atur menjadi sebelum waktu saat ini.

Di Laravel kita melakukan hal tersebut dengan menggunakan method `withoutCookie(name)`.