Di PHP biasanya ketika kita ingin mendapatkan detail dari request kita lakukan menggunakan global variable seperti `$_GET`, `$_POST` dan lain-lain.

Di Laravel kita tidak perlu melakukan itu lagi, HTTP Request dibungkus dalam sebuah object dari class `Illuminate\Http\Request`.

Dan kita bisa menambahkan type-hint class `Illuminate\Http\Request` pada Closure Route atau Controller method dan nanti secara otomatis Laravel service container akan meng-inject object Request-nya.

## Request Input

Untuk mengambil input user kita bisa menggunakan method `input(key, default)` pada Request, dimana jika keynya tidak maka akan mengembalikan default value yang kita masukkan pada argument kedua dari method-nya, tetapi jika kita tidak memasukkan default value, maka akan mengembalikan `null` dan kita tidak perlu memperdulikan HTTP method apa yang digunakan dari request dan dari mana asalanya, entah dari body atau query parameter.

Method `input()` ini pertama akan mengambil data dari query parameter, kemudian dari body, jika data yang ada di query param ada juga di body dengan key yang sama, maka data yang ada di body akan mereplace data yang ada di query param.

## Nested Input

Salah satu fitur powerful di Laravel adalah kita menerima input nested hanya dengan menggunakan notasi titik `.`, misal jika kita menggunakan `$request->input('name.first')`, maka itu artinya mengambil key `first` di dalam  `name`. Ini cocok ketika kita mengirim request dalam bentuk JSON atau form (jika kita mengirim datanya dalam bentuk array).

## Mengambil semua Input

Jika ingin mengambil semua input yang terdapat di HTTP Request, kita bisa menggunakan method `input()`, tapi tanpa argument apapun dan nilai input yang dihasilkan nantinya adalah associative array.

## Mengambil Array Input

Laravel juga memilki kemampuan mengambil value dari input berupa array. Misal kita bisa menggunakan `$request->input('products.*.name')`, artinya kita mengambil semua `name` yang ada di array products. Atau kita bisa juga menggunakan `$request->input('products.0.name')`, artinya kita mengambil name dari product index ke-0 yang ada di array products.

## Input Query Parameter

Kita tahu bahwa `input()` itu mengambil semua input baik dari query param atau body, namun jika kita ingin megambil input hanya dari query parameter, kita bisa menggunakan method `$request->query(key)` atau jika ingin mengambil semua input query dalam bentuk associative array kita bisa menggunakan `$request->query()` tanpa argument apapun.

## Dynamic Properties

Laravel juga mendukung Dynamic Properties yang secara otomatis akan mengambil value dari input Request. Misalnya di form aplikasi kita miliki input dengan `name="age"`, maka untuk mengakses valuenya, lakukan seperti ini:

```php
$age = $request->age;
```

Ketika menggunakan Dynamic Properties, Laravel pertama akan melihat nilai parameter yang ada di request payload (request body), jika tidak ada, maka Laravel akan mencari field dengan nama yang sama di Route parameter.

Walaupun bisa menggunakan Dynamic Property, tetapi lebih disarankan untuk menggunakan method seperti `input()`, `query()` dan sebagainya, karena jika sewaktu-waktu di object Request ada yang menambahkan property yang nama key nya sama dengan nama key dari data yang kita kirim, otomatis dia tidak akan mengambil lagi dari nilai input yang kita kirim.