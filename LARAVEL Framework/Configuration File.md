Selain environment variable, Laravel juga mendukung konfigurasi dengan menggunakan PHP Code, konfigurasi ini digunakan ketika memang dibutuhkan konfiguasi yang tidak terlalu sering berubah dan biasanya pengaturannya hampir sama untuk disetiap lokasi aplikasi dijalankan.

Ketika menggunakan fitur Laravel Configuration, kita juga tetap bisa mengakses Environment Variable karena keduanya terintegrasi dengan baik (lihat contoh [[#Membuat File Konfigurasi]]).

## Folder Configuration

Laravel menyimpan semua konfigurasi di folder `config`, dan prefix (awalan) nama dari konfigurasi diawali dengan nama file yang terdapat dalam folder tersebut. Sebagai contoh di folder `config` ada file `app.php`, maka nama awal dari konfigurasi akan seperti ini `APP_`, seperti `APP_NAME` dan sebagainya. Jika nama file berubah maka prefix-nya juga berubah.

> Maksud dari nama *konfigurasi* dalam konteks ini adalah, tidak ada kaitannya dengan nama konfigurasi pada environment variable dan file `phpunit.xml`, nama konfigurasi ini merujuk kepada ketika kita ingin mengambil nilai dari konfigurasi.  
<p></p>

## Membuat File Konfigurasi

Untuk membuat file konfigurasi kita cukup membuat file konfigurasi di dalam folder `config`. Lalu di dalam file tersebut kita cukup `return`  konfigurasi dalam bentuk array.

Disarankan untuk nama file menggunakan huruf kecil semua, supaya lebih gampang ketika mau mengambil nilai dari konfigurasinya, karena nama konfigurasi itu *casesensitive*.

Contoh:

```php
<?php

return [
	'author' => [
        'first' => env('AUTHOR_FIRST', 'Reza'),
        'last' => env('AUTHOR_LAST', 'Sariful Fikri')
    ],
    'email' => 'fikkri.reza@gmail.com',
    'web' => 'rezafikkri.github.io',
];
```

## Mengambil Konfigurasi

Untuk mengambil nilai konfigurasi di file konfigurasi bisa menggunakan function `config(key, default)`. Dimana `key` adalah kabungan dari nama file konfigurasi, lalu titik (.) dan diikuti dengan key yang terdapat pada return value-nya. Tiap nested array menggunakan titik (.). Sama seperti function `env()`, function `config()` juga memiliki parameter default, jika key konfigurasinya tidak tersedia.

Contoh:

```php
<?php

$authorFirst = config('contoh.author.first');
$email = config('contoh.email');
```

> Menurut pemahaman saya, config di Laravel utamanya adalah apa yang ada dalam folder `config`, sedangkan environment variable yang terdapat pada `.env` adalah pelengkap dari config tersebut, supaya lebih memudahkan kita jika ada konfigurasi yang butuh berubah-ubah sesuai dengan environmentnya.