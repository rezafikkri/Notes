Di PHP biasanya ketika kita ingin mendapatkan detail dari request kita lakukan menggunakan global variable seperti `$_GET`, `$_POST` dan lain-lain.

Di Laravel kita tidak perlu melakukan itu lagi, HTTP Request dibungkus dalam sebuah object dari class `Illuminate\Http\Request`.

Dan kita bisa menambahkan type-hint class `Illuminate\Http\Request` pada Closure Route atau Controller method dan nanti secara otomatis Laravel service container akan meng-inject object Request-nya.

## Request Input

Untuk mengambil input user kita bisa menggunakan method `input(key, default)` pada Request, dimana jika keynya tidak maka akan mengembalikan default value yang kita masukkan pada argument kedua dari method-nya dan kita tidak perlu memperdulikan HTTP method apa yang digunakan dari request.

## Nested Input

Salah satu fitur powerful di Laravel adalah kita menerima input nested hanya dengan menggunakan notasi titik `.`, misal jika kita menggunakan `$request->input('name.first')`, maka itu artinya mengambil key `first` di dalam  `name`. Ini cocok ketika kita mengirim request dalam bentuk JSON atau form (jika kita mengirim datanya dalam bentuk array).