Facades adalah class yang menyediakan static akses ke class atau fitur yang tersedia di dalam Service Container atau Application. Laravel dilengkapi dengan banyak facades yang menyediakan hampir ke semua fitur Laravel.

Facades sangat membantu misalnya pada kasus ketika kita membuat kode class yang bukan bawaan fitur Laravel.

## Kapan Menggunakan Facades?

Gunakan facades jika memang dibutuhkan saja, jika bisa dilakukan menggunakan dependency injection, gunakan dependency injection.

Terlalu banyak menggunakan facades akan membuat kita tidak sadar bahwa sebuah class banyak sekali memiliki dependency, karena sebenarnya ketika kita menggunakan facades, kita akan membuat dependency (ketergantungan) ke tempat lain, sedangkan jika kita menggunakan dependency injection, kita bisa sadar dengan banyaknya parameter yang terdapat di constructor, tetapi kalau kita menggunakan facades tidak akan terlihat, karena kita menggunakan static function untuk mengakses dependencynya.

## Facades vs Helper Function

Di Laravel selain facades ada juga Helper Function,  bedanya pada Helper Function tidak dikumpulkan dalam class. Sebagai contoh sebelumnya kita telah menggunakan function `config()` dan `env()`, itu adalah Helper function yang ada di Laravel, yang sebenarnya di dalamnya memanggil facades. Penggunaan helper function sebenarnya lebih mudah, namun jika dibandingkan dengan Facades, maka penggunaan Facades akan lebih mudah dimengerti secara kode.

```php
$firstName1 = config('contoh.author.first');
$firstName2 = Config::get('contoh.author.first');
```

## Bagaimana Facades Bekerja?

Facades adalah sebenarnya class yang menyediakan akses ke dalam dependency yang terdapat di dalam Service Container. Jadi sebenarnya di dalam Service Container itu ada yang namanya object dependency `Config`, dengan menggunakan Facades sebenarnya kita mengambil dependency yang bernama `Config` di dalam Service Container.

Semua class Facades adalah turunan dari class `Illuminate\Support\Facades\Facade`, class Facade menggunakan magic method yang bernama `__callStatic`, yang mana method tersebut akan di jalankan ketika kita memanggil static method di Facade dan akan meneruskan secara otomatis ke dependency yang ada di Service container. Contoh `Config::get()` sebenarnya akan melakukan pemanggilan method `get()` yang ada di dependency atau object Config di Service Container.

Didalam class Fecade Config atau class-class Fecade yang lain, ada method `getFacadeAccessor()`, yang mana method ini mengembalikan nama dependency yang terdapat di Container, atau bisa disebut nama dari Service Container binding.

```php
$config = $this->app->make('config');
$firstName1 = $config->get('contoh.author.first');

// kode dibawah ini sebenarnya,
// melakukan hal yang sama seperti kode di atas:
// yaitu pertama mengambil instance class/dependency 'config'
// yang ada di dalam Service Container,
// kemudian menjalankan method 'get()'.
//
// kata kunci 'config' diatas adalah nama dari Service Container
// Binding, atau bisa juga disebut nama dependency
// yang terdapat di Container
$firstName2 = Config::get('contoh.author.first');
```

Jadi kita menggunakan Facade ketika kita tidak bisa mengakses si `app` atau Application.

## Facades Mock

Salah satu kekurangan menggunakan static function biasanya sulit untuk ditest, karena mocking static function sangat sulit. Namun untungnya di Laravel sudah disediakan function untuk melakukan mocking di Facades, sehingga kita bisa mudah ketika implementasi unit test. Laravel menggunakan [Mockery](https://github.com/mockery/mockery) untuk melakukan Mocking Facades. Jadi misalnya ketika kita membuat unit test dan kita tidak mau memanggil object dari dependencynya, kita bisa mock, atau membuat object tiruannya.

```php
Config::shouldReceive('get')
	->with('contoh.author.first')
	->andReturn('RezaFikkri');
```

## Daftar Facades

Ada banyak facade di Laravel dan seperti yang dijelaskan sebelumnya, hampir semuanya banyak menggunakan dependency di Service Container. Jadi apa yang ada di Facades itu sebenarnya isinya adalah dependency yang ada di dalam si Containernya.

https://laravel.com/docs/11.x/facades#facade-class-reference