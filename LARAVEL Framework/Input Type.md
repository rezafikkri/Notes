Class Request di laravel memiliki beberapa helper method yang digunakan untuk melakukan konversi input secara otomatis.

Ini bisa digunakan untuk mempermudah kita ketika ingin otomatis melakukan konversi input data ke tipe data yang kita inginkan, karena walaupun kita mengirim datanya misalnya yang kita harapkan adalah integer, boolean, tetapi sebenarnya yang kita kirim adalah berupa string.

Beberapa methodnya adalah:
1. `boolean(key, default)`; untuk melakukan konversi tipe data input secara otomatis ke boolean.
2. `date(key, pattern, timezone)`; untuk melakukan konversi tipe data input ke Date secara otomatis. Laravel menggunakan library [Carbon](https://github.com/CarbonPHP/carbon) untuk manipulasi tipe data Date dan Time. `pattern` adalah format dari data input date yang kita kirim.
3. `integer(key, default)`; untuk melakukan konversi tipe data input secara otomatis ke integer.