> **Bootstrapped** atau bootstrap di Service Provider pada Laravel adalah meregistrasikan (mendaftarkan) hal-hal, termasuk meregistrasikan Service Container binding, event listeners, middleware dan bahkan routes.

Sekarang kita tahu cara untuk melakukan dependency injection di Laravel, sekarang pertanyaannya apakah ada best practice dimana melakukan dependency injection tersebut?

Laravel menyediakan fitur bernama Service Provider, dari namanya kita sudah tahu bahwa ini adalah penyedia service atau dependency.

Di dalam Service Provider biasanya kita melakukan registrasi dependency di dalam Service Container, yaitu seperti yang sebelumnya kita lakukan menggunakan method `bind`, `singleton` dsb.

Service Provider adalah tempat utama (central place) untuk mengkonfigurasikan aplikasi kita.

## Membuat Service Provider

Untuk membuat Service Provider gunakan perintah:

```bash
php artisan make:provider NamaServiceProvider
```

## Service Provider Method

Di dalam Service Provider terdapat 2 method yaitu:

- **`register`**: tempat melakukan registrasi dependency (binding) yang dibutuhkan ke Service Container. Jangan melakukan registrasi event listener, route, atau bagian funsionalitas lainnya di dalam method ini, jika tidak kita mungkin secara tidak sengaja menggunakan sebuah service yang di sediakan oleh Service Provider yang belum di load.

- **`boot`**: method ini akan dipanggil setelah semua service provider lainnya diregistrasi. Disini kita bisa melakukan apapun yang diperlukan setelah proses registrasi dependency selesai.

## Registrasi Service Provider

Setelah kita membuat Service Provider, secara default jika kita membuat Service Provider secara manual, maka Service Provider yang kita buat itu tidak akan diload oleh Laravel. Maka dari itu kita perlu menambahkan Service Provider tersebut ke array yang ada di file `bootstrap/providers.php`. Tetapi jika kita membuat Service Provider dengan menggunakan perintah:

```bash
php artisan make:provider NamaServiceProvider
```

Maka Laravel secara otomatis akan menambahkan provider yang telah dibuat ke dalam file `bootstrap/providers.php`.

## Bindings and Singletons Properties

Jika kita hanya butuh melakukan binding serderhana, misal dari interface ke class, kita bisa menggunakan fitur binding via properties di Service Provider.

Kita bisa menggunakan property `bindings` untuk membuat binding, atau menggunakan property `singletons` untuk membuat binding singleton.

Namun ini hanya untuk kasus sederhana, jika kasusnya kompleks, misalnya pada constructor class tersebut butuh tipe data string, maka tidak bisa menggunakan fitur ini.

## Deffered Provider
