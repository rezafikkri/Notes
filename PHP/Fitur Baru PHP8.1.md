## Enumerations

Adalah tipe data yang nilainya terbatas, atau yang telah kita tentukan nilainya di awal ketika kita membuat kode programnya. Dengan Enum kita bisa membuat tipe data kita sendiri, sesuai kebutuhan, misalnya Enum ini bisa kita gunakan untuk membuat tipe data Gender, Level dan sejenisnya. Contoh:

```php
<?php

enum Gender
{
    case Male;
    case Female;
}
```

### Backed Enumerations

Kadang, saat membuat Enum kita ingin data Enum tersebut mewakili atau merepresentasikan data lainnya, misalnya string atau integer. Contoh kasusnya adalah kadang-kadang kita ingin di kode programnya adalah Enum, tetapi ketika disimpan ke database kita ingin datanya menjadi string. Secara default ini tidak bisa, kita perlu menambahkan titik dua setelah nama Enum dan diikuti oleh tipe datanya. Contoh:

```php
enum Gender: string
{
    case Male = 'Mr';
    case Female = 'Mrs';
}
```

Sebuah Enum yang semua case-nya memiliki _Backed_, disebut sebagai _Backed Enum_. Sebuha Backed Enum hanya bisa memiliki Backed cases, sedangkan sebuah Pure Enum (Enum yang semua case-nya tidak memiliki Backed) hanya bisa memiliki Pure Cases.

Salah satu keuntungan menggunakan Backed Enum adalah, kita bisa melakukan konversi tipe data Enum-nya ke tipe data Backed-nya, ataupun sebaliknya juga bisa.

### Enumerations Method

Enum mirip seperti class, dia bisa memiliki method atau function.

### Enumerations Inheritance

Enum juga bisa melakukan pewarisan, tetapi terbatas hanya dengan interface dan trait, tidak bisa dengan class ataupun enum yang lain.

### Enumerations Constant

Enum bisa memiliki contant, tetapi tidak memilik property atau attribute.

## Readonly Property

Pada php8.1 terdapat kata kunci `readonly` yang bisa digunakan pada property, yang mana ini bisa digunakan untuk mencegah modifikasi dari property setelah inisialisasi. Readonly property hanya bisa ditentukan datanya sekali dari scope dimana dia dideklarasikan dan selanjutnya dia tidak bisa diubah lagi. Readonly Property tidak bisa memiliki default value. Kita bisa saja membuat setter method untuk inisialisasi nilai dari Readonly Property, cuman lebih disarankan menggunakan Constructor untuk inisialisai nilai dari Readonly Property.

Contoh:

```php
<?php

class Category
{
    public function __construct(
        public readonly string $id,
        public readonly string $name,
    ) {
    }
}

$category = new Category("1", "Handphone")
```

## First Class Callable Syntax

Di PHP8.1 kita bisa membuat reference ke function secara mudah hanya dengan menggunakan tanda titik tiga kali (...).

## New In Initializer

Sekarang Object bisa digunakan sebagai default value dari parameter ketika kita membuat function.

## Pure Intersection Type

Dengan fitur ini kita bisa membuat properties atau parameter yang wajib sesuai dengan beberapa tipe data. Ini cocok ketika kita ingin menentukan misal parameter harus merupakan tipe data turunan beberapa interface. Jika Union Type menggunakan karakter |, sedangkan Intersection Type menggunakan karakter &.

## Never Return Type

`never` return type mengindikasikan bahwa sebuah function/method tidak akan pernah mengembalikan suatu nilai dan harus selalu _throw an exception_ atau diakhiri dengan pemanggilan `die`/`exit`.

Never return type ini cocok digunakan ketika kita ingin membuat function yang setelah dia dijalankan kita ingin menghentikan eksekusi kode program phpnya.

Never ini sebenarnya bisa dibilang untuk penanda bahwa jika suatu function memiliki return type `never`maka sudah dipastikan jika memanggil function tersebut maka kode setelahnya tidak akan dijalankan.