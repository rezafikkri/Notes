Sebelumnya kita sudah mencoba melakukan dependency injection secara manual di [[Dependency Injection]]. Laravel memiliki fitur dependency injection secara otomatis dan ini wajib dikuasasi agar lebih mudah membuat aplikasi menggunakan Laravel. Di Laravel fitur ini bernama **Service Container**, dimana Service Container ini merupakan fitur yang digunakan untuk management semua dependencies yang ada di Laravel dan juga dependency injection.

## Application Class

Service Container di Laravel direpresentasikan dalam class bernama Application. Application adalah Container atau Manager dari dependency injectionnya. Kita tidak perlu membuat class Application secara manual, karena semua sudah dilakukan secara otomatis oleh Laravel. Di semua project Laravel hampir semua bagian terdapat field `$app` yang merupakan instance dari Application.

## Membuat Dependency

Dengan menggunakan Service Container kita tidak perlu lagi membuat object secara manual menggunakan kata kunci `new`. Kita bisa menggunakan function `make(key)` yang terdapat di class Application untuk membuat dependency secara otomatis. Saat kita menggunakan `make(key)` , object akan selalu dibuat baru, jadi harap hati-hati ketika menggunakannya, karena dia bukan menggunakan object sama.

```php
$foo = $this->app->make(Foo::class); // new Foo
$foo2 = $this->app->make(Foo::class); // new Foo
```

## Mengubah Cara Membuat Dependency

Saat kita menggunakan function `make(key)`, secara otomatis Laravel akan membuat objectnya, namun kadang kita ingin menentukan cara pembuatan objectnya. Misalnya pada object yang kompleks, ada constructor nya ada banyak parameternya.

Pada kasus seperti ini kita bisa menggunakan method `bind(key, closure)`, kita cukup returnkan data yang kita inginkan pada function `closure`nya. Saat kita menggunakan `make(key)` untuk mengambil dependencynya, secara otomatis function `closure` akan dipanggil. Perlu diingat juga bahwa setiap kita memanggil `make(key)` function `closure`nya akan selalu dipanggil, jadi bukan menggunakan object yang sama.