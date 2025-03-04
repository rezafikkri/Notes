Di dalam file `config/app.php` ada option `debug` yang menentukan bagaimana error ditampilkan kepada user. Selama dev disarankan nilai dari `debug` ini adalah `true`.

## Ignore Exception

Kadang ada kasus ketika terjadi error, kita tidak ingin melakukan report error tersebut, contoh jika error validasi kita tidak perlu melakukan report. Untuk melakukan itu kita menambahkan jenis exception yang tidak ingin di report di method `withExceptions`  dengan method `dontReport` di file `bootstrap/app.php`. Atau kita bisa menandai exception nya dengan interface `Illuminate\Contracts\Debug\ShouldntReport`.

## Rendering Exception

Secara default, Laravel akan melakukan render halaman error ketika terjadi exception, namun jika kita mau, kita juga bisa membuat halaman web sendiri ketika terjadi exception. Untuk mendaftarkannya, kita bisa gunakan method  `render` di dalam method `withExceptions` di dalam file `bootstrap/app.php`. method `render` harus me return instance dari `Illuminate\Http\Response`.

Contoh ketika terjadi validation error, kita ingin menampilkan halaman web Bad Request dan HTTP Status 400 misalnya.

