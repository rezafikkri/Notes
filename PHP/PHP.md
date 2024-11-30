## `if` vs `switch` vs `match`

Secara umum: Opsi pertama adalah menggunakan ternary operator, jika kondisi yang ingin di cek sederhana. Jika kondisi lebih banyak bisa menggunakan `match`, jika butuh _multiple expresssion in code block_ misalnya menjalankan dua fungsi sekaligus ketika suatu kondisi terpenuhi, maka bisa menggunakan `switch`, dan ketika butuh untuk membandingkan banyak variable dengan banyak value, maka bisa menggunakan `if`.

## Polymorphisme

Adalah kemampuan sebuah object berubah bentuk menjadi bentuk lain. Hal ini berkaitan dengan inheritance.

Atau bisa juga diartikan, kemampuan suatu objek untuk mengambil bentuk yang berbeda dan menunjukkan perilaku yang berbeda sambil berbagi interface yang sama. Ini membuat class yang berbeda bisa mengimplementasikan nama method yang sama dengan fungsi yang berbeda.

## Entity atau Model

Sebenarnya sama, yaitu class representasi table yang ada di database (jika di php sering disebut Model, terutama di framework2 seperti codeigniter dan laravel).
![Repository Pattern](https://ik.imagekit.io/rezafikkri/Repository%20Pattern.png?updatedAt=1732926544634)
*Diagram Repository Pattern*

## Getter dan Setter

Ini dilakukan supaya data dalam sebuah object tetap terjaga (baik dan valid) dan tidak gampang berubah. memang kelihatannya sedikit ribet, tapi dengan ini misalnya kita bisa menambahkan validasi pada method setter. dsb.

Standar yang biasanya digunakan:

| **Tipe Data** | **Getter Method** | **Setter Method** |
| ------------- | ----------------- | ----------------- |
| Boolean       | isXxx()           | setXxx()          |
| Lainnya       | getXxx()          | setXxx()          |

## Composer

Perintah `composer update` digunakan ketika pertama kali membuat project dan tidak ada file composer.lock, selain itu juga digunakan untuk meng-update dependency, baik secara otomatis dengan menjalankan `composer update`, atau mengubah versi dependency yang ada di file composer.json (biasanya ketika ingin mengupdate _major version_ dari dependency).

Perintah `composer install` digunakan ketika kita clone project dari github misalnya dan biasanya sudah terdapat file composer.lock.

Perintah `composer dump-autoload` digunakan untuk mengupdate file autoload.php, ketika kita mengupdate autoload field di file composer.json. Dan kita tidak perlu menjalankan lagi perintah `composer dump-autoload` setelah menjalankan perintah `composer require` dan `composer update` ketika menambahkan package baru, baik secara manual (meng-edit field require di file composer,json), maupun otomatis dengan `composer require`.

## Function atau Method

Parameter dengan default value harus di tempatkan pada urutan terakhir, tidak boleh setelah parameter degan default value lalu membuat parameter tanpa default value.

Contoh:
```php
<?php

function sayHello($first,$two,$third='default') {}
```

### Covariance & Contravariance

Covariance: fitur di php7.4 yang memungkinkan kita membuat return type yang lebih spesifik pada child method, dari pada yang ada pada parent method.

Contravariance: fitur di php7.4 yang memungkinkan kita membuat parameter type yang lebih tidak spesifik pada child method dari pada yang ada pada parent method.

### HTTP Header

Untuk header pastikan menambahkannya sebelum kita membuat content. Karena pada spesifikasi HTTP letak header adalah sebelum content atau body

### Virtual Host

Merujuk kepada praktik menjalankan lebih dari satu website di dalam satu mesin. Caranya adalah dengan membuat beberapa domain dalam satu server yang sama.

---

:TiNotes:  [[PHP MVC]]
:TiNotes:  [[PHP Logging]]
:TiNotes:  [[Fitur Baru PHP8.1]]
:TiNotes:  [[PHP Standard Recomendation]]
