Service Provider adalah tempat utama dari semua bootstrapping aplikasi Laravel. Aplikasi yang kita buat, maupun semua code service dari Laravel, di bootstrapped melalui Service Provider.

Tetapi atap itu Bootstrapped? **Bootstrapped** atau bootstrap di Service Provider pada Laravel adalah meregistrasikan (mendaftarkan) hal-hal, termasuk meregistrasikan Service Container binding, event listeners, middleware dan bahkan routes. Service Provider adalah tempat utama (central place) untuk mengkonfigurasikan aplikasi kita.

Sebelumnya kita sudah tahu cara untuk melakukan dependency injection di Laravel, sekarang pertanyaannya apakah ada best practice dimana melakukan dependency injection tersebut?

Laravel menyediakan fitur bernama Service Provider, dari namanya kita sudah tahu bahwa ini adalah penyedia service atau dependency.

Di dalam Service Provider biasanya kita melakukan registrasi dependency di dalam Service Container, yaitu seperti yang sebelumnya kita lakukan menggunakan method `bind`, `singleton` dsb.

## Membuat Service Provider

Untuk membuat Service Provider gunakan perintah:

```bash
php artisan make:provider NamaServiceProvider
```

## Service Provider Method

Di dalam Service Provider terdapat 2 method yaitu:

- **`register`**: tempat melakukan registrasi dependency (binding) yang dibutuhkan ke Service Container. Jangan melakukan registrasi event listener, route, atau bagian funsionalitas lainnya di dalam method ini, jika tidak kita mungkin secara tidak sengaja menggunakan sebuah service yang di sediakan oleh Service Provider yang belum di load.

- **`boot`**: method ini akan dipanggil setelah semua service provider lainnya diregistrasi. Disini kita bisa melakukan apapun yang diperlukan setelah proses registrasi dependency selesai.

## Registrasi Service Provider

Setelah kita membuat Service Provider, secara default jika kita membuat Service Provider secara manual, maka Service Provider yang kita buat itu tidak akan diload oleh Laravel. Maka dari itu kita perlu menambahkan Service Provider tersebut ke array yang ada di file `bootstrap/providers.php`. Tetapi jika kita membuat Service Provider dengan menggunakan perintah:

```bash
php artisan make:provider NamaServiceProvider
```

Maka Laravel secara otomatis akan menambahkan provider yang telah dibuat ke dalam file `bootstrap/providers.php`.

## Bindings and Singletons Properties

Jika kita hanya butuh melakukan binding serderhana, misal dari interface ke class, kita bisa menggunakan fitur binding via properties di Service Provider.

Kita bisa menggunakan property `bindings` untuk membuat binding, atau menggunakan property `singletons` untuk membuat binding singleton.

Namun ini hanya untuk kasus sederhana, jika kasusnya kompleks, misalnya pada constructor class tersebut butuh tipe data string, maka tidak bisa menggunakan fitur ini.

## Deffered Provider

Secara default semua Service Provider akan diload oleh Laravel, baik itu dibutuhkan atau tidak. Nah, jika provider yang kita buat itu hanya meregistrasikan binding dalam Service Container, maka kita bisa menandai Service Provider tersebut agar hanya diload jika benar-benar dibutuhkan dengan menggunakan fitur Deffered Provider yang ada di Laravel.

Kita bisa menandai Service Provider kita dengan implement interface `DefferableProvider`, lalu implement method `provides`, yang mengembalikan binding Service Container yang diregistrasi oleh provider kita.

Dengan fitur ini, Service Provider hanya akan diload ketika class atau interface atau keduanya bisa disebut juga dependency yang kita binding memang dibutuhkan. Setiap ada request baru, maka Service Provider yang sudah Deffered tidak akan diload jika memang tidak dibutuhkan.

```php
<?php

...
use Illuminate\Contracts\Support\DeferrableProvider;
use Illuminate\Support\ServiceProvider;

class FooBarServiceProvider extends ServiceProvider implements DeferrableProvider
{
    public array $singletons = [
        HelloService::class => HelloServiceIndonesian::class,
    ];

    /**
     * Register services.
     */
    public function register(): void
    {
        $this->app->singleton(Foo::class, function () {
            return new Foo;
        });

        $this->app->singleton(Bar::class, function (Application $app) {
            return new Bar($app->make(Foo::class));
        });
    }

    /**
     * Bootstrap services.
     */
    public function boot(): void
    {
        //
    }

    public function provides(): array
    {
        return [
            HelloService::class,
            Foo::class,
            Bar::class,
        ];
    }
}
```

Jadi provider yang kita buat seperti contoh diatas hanya akan diload ketika kita misalnya memanggil `Foo::class` atau `Bar::class` atau `HelloService::class`, jika salah satu diantara tiga dependency itu tidak ada yang dipanggil, maka ServiceProvider diatas tidak akan diload.