>Encryption adalah sebuah proses dimana data (text, password atau file) diubah kedalam format yang tidak bisa dibaca, yang disebut *chipertext*, menggunakan algoritma matematika dan sebuah secret key.

Laravel memiliki abstraction feature untuk melakukan encryption, dengan ini kita tidak perlu melakukan enkrip dan dekrip secara manual.

Untuk melakukan enkripsi Laravel membutuhkan key, dimana key tersebut disimpan di `config/app.php`. Secara default Laravel akan mengambil key tersebut dari environment `APP_KEY`, kita bisa cek di file `.env`.

## Membuat Encryption Key

Key untuk enkripsi hendaknya dibuat secara random dan diubah secara berkala, misalnya 1 bulan sekali, supaya lebih aman dan jangan terlalu sering mengubahnya, misalnya 1 hari sekali, karena karena beberapa fitur di Laravel menggunakan key tersebut untuk melakukan enkripsi. Salah satu yang menggunakannya adalah session cookie, jika kita mengubah keynya setiap hari, maka setiap hari session cookie nya akan invalid dan semua user yang sudah login akan terlogout. Kenapa bisa tidak valid lagi? karena tidak memungkinkan untuk deskrip data apapun yang telah di enkrip tanpa kita mengetahui key nya.

Tetapi kita tidak perlu khawatir, Laravel sudah memiliki cara untuk mengurangi masalah tersebut, yaitu dengan fitur *[Gracefully Rotating Encryption Keys](https://laravel.com/docs/11.x/encryption#gracefully-rotating-encryption-keys)* , dimana kita bisa menyimpan semua key enkripsi kita sebelumnya di dalam environment variable `APP_PREVIOUS_KEYS` dan key yang ada dalam environment variable tersebut dipisahkan oleh koma.

Kita bisa membuat key menggunakan perintah:

```bash
php artisan key:genereate
```

Untuk menyimpan enkripsi key sebelumnya, kita harus melakukannya secara manual sebelum menjalankan perintah diatas, karena jika kita tidak melakukan seperti itu, maka Laravel akan me-replace key sebelumnya (*otomatis kita akan kehilangan key sebelumnya*) dan dampaknya semua user yang sudah login akan terlogout.

## Dekrip dan Enkrip

Untuk melakukan enkrip dan dekrip kita bisa menggunakan Facade `Crypt`.