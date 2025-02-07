Kadang saat kita menerima input data dari user, kita ingin secara mudah menerima semua key input, lalu menyimpannya ke database misalnya.

Pada kasus seperti ini kadang sangat berbahaya jika misal user secara sengaja mengirim key yang salah, lalu kita mencoba melakukan update key yang salah itu ke database. Misalnya kita ada aksi menyimpan data ke database dan kita menerima datanya dari form, nah jangan sampai misalnya ada orang yang mengirim key `admin` dengan nilai true dan dia tiba-tiba menjadi admin, padahal bukan admin.

Untungnya Laravel memiliki helper method di class Request untuk melakukan filter input, misalnya kita ingin mengambil semua data input, tetapi jika ada data key yang lain kita mau filter atau dibuang.

Method yang bisa digunakan adalah:
1. `$request->only([key1, key2])`; akan mengembalikan hanya data key yang kita sebutkan di argument, namun jika salah satu key misalnya tidak di kirim melalui request, key tersebut tidak akan dikembalikan. Jadi misalnya pada argument method kita masukkan key `username` dan `password`, jika password tidak kita kirimkan melalui request, maka pada array hasil return method `onyl()` nantinya tidak akan ada `password`.
2. `$request->except([key1,key2])`; digunakan untuk mengambil semua input, tapi tidak dengan yang kita sebutkan di argument.
Kedua method diatas selain menerima array sebagai argumentnya, kedua method itu juga menerima daftar argument dinamis, contoh:

```php
$input = $request->only('username', 'password');
$input = $request->except('admin');
```

## Merge Additional Input

Kadang-kadang kita butuh untuk menambahkan secara manual input kedalam request.

Untuk melakukan ini kita bisa menggunakan method `$request->merge(array)`, untuk menambahkan input ke request dan jika ada key yang sama, maka data tersebut akan ditimpa dengan argument yang kita masukkan ke method `merge()`.

Atau kita juga bisa menggunakan method `$request->mergeIsMissing(array)`, untuk menambah input ke request dan jika sudah ada input dengan key yang sama, maka akan di batalkan.